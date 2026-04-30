---
title: 分割付款POC逐步實作指南
description: 瞭解如何在設定完成之後依序實作分割付款POC，範圍涵蓋模組、環境、協調器、驗證和選用的UI。
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# 分割付款POC逐步實作指南

此頁面是您的實施檢查清單。 此影片假設您已觀看[總覽](./overview.md)和[完整示範](./full-demo.md)，且您的Commerce網站和App Builder專案都已準備就緒。 個別教學課程頁面包含下列每個主題的完整詳細資訊 — 使用此頁面來瞭解該做什麼以及按照什麼順序。

## 開始之前

在執行任何提示或命令之前，請先確認下列所有專案。 [必要條件與設定](./prerequisites-and-setup.md)頁面詳細涵蓋每個專案。

**Commerce端**

* [ ] Adobe Commerce 2.4.5或更新版本
* [ ] I/O事件已啟用且已部署（僅限Commerce Cloud — 需要`.magento.env.yaml`中的`ENABLE_EVENTING: true`）
* [ ] **貨到付款**&#x200B;已在Admin中啟用，其標題設定為`Cash`
* [ 已在Admin中啟用]商店點數
* [ ]測試客戶至少有$50美元的商店點數
* [ ] Commerce整合已建立、啟用，而且四個OAuth值都已安全儲存

**App Builder端**

* [ Adobe Developer Console中有]個App Builder專案
* [ ]個Adobe I/O Events服務已新增至工作區
* [ ]個Commerce已連線為事件提供者
* [ ] `aio login`完成；使用`aio app use`選取了正確的工作區
* [ 已安裝] Node.js 18或更新版本； `aio` CLI已安裝(`npm install -g @adobe/aio-cli`)

如果缺少上述任何專案，請在此處停止，然後先完成它。

## 階段1 — 建立Commerce模組

**這會做什麼：**&#x200B;產生`Client_SplitPayment` PHP模組，處理簽出UI、儲存信用申請、REST端點和I/O事件訂閱。 這是精簡的Commerce內配接卡 — 所有運運算元工作流程都保留在App Builder中。

### 步驟1.1 — 自訂專案的提示

開啟[Commerce模組AI提示](./commerce-module-prompt.md)頁面，並在複製提示之前記下下列內容：

* 在提示中將`Client`取代為您的真實廠商名稱
* 如果您想要不同的模組名稱，請取代`SplitPayment`
* 如果您的主題不是Luma，`LayoutProcessorPlugin`插入路徑和RequireJS對應可能需要調整
* 若您的貨到付款方式使用`cashondelivery`以外的代碼，請更新`payment-method-helper.js`

### 步驟1.2 — 執行提示

將完整的提示從&#x200B;**PROMPT START**&#x200B;複製到&#x200B;**提示結束**，然後貼到Cursor （使用Claude）或直接貼到Claude中。 從您Commerce專案的根目錄執行，讓AI可以在`app/code/`下建立檔案。

### 步驟1.3 — 啟用並安裝模組

從您的Commerce專案根目錄執行以下命令：

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 步驟1.4 — 確認模組已正確安裝

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

您應該會看到五個資料行： `split_store_credit_amount`、`split_cash_amount`、`split_cash_status`、`split_sc_invoice_id`以及`split_cash_invoice_id`。

然後在瀏覽器中確認，當擁有商店信用額的登入客戶選取&#x200B;**現金**&#x200B;作為付款方式時，分割付款欄位會在結帳時顯示。

## 階段2 — 設定環境變數

**這會做些什麼：**&#x200B;準備App Builder Orchestrator、模擬指令碼和（選擇性） Experience Cloud UI擴充功能所使用的認證檔案。 來自您Commerce整合的四個OAuth值會用於每個`.env`檔案中。

如需每個元件的完整變數清單，請參閱[環境變數參考](./env-reference.md)。

### 步驟2.1 — 建立協調器`.env`

在您的`split-payment-orchestrator/`目錄中：

```bash
cp .env.example .env
```

填入每個值：

| 變數 | 從何處取得 |
|---|---|
| `COMMERCE_BASE_URL` | 您的商店URL — 無結尾斜線 |
| `COMMERCE_CONSUMER_KEY` | Commerce管理員>系統>整合 |
| `COMMERCE_CONSUMER_SECRET` | 相同 — 僅在啟動時顯示 |
| `COMMERCE_ACCESS_TOKEN` | 相同 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 相同 |
| `PAYMENT_THRESHOLD` | 必須與Commerce中的`split_payment/general/threshold`相符（預設： `100`） |
| `DEMO_UI_SECRET` | 選擇性 — 若已設定，則為儀表板URL中的`?secret=`所必需 |

### 步驟2.2 — 建立模擬指令碼`.env`

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

填入`COMMERCE_BASE_URL`和四個OAuth值（與上述認證相同）。

## 階段3 — 建立App Builder Orchestrator

**這會做些什麼：**&#x200B;產生`split-payment-orchestrator` App Builder專案： `payment-orchestrator` I/O事件取用者、`payment-accept`和`payment-decline` Web動作，以及`demo-dashboard`運運算元UI。

### 步驟3.1 — 執行Orchestrator提示

開啟[App Builder orchestrator AI提示字元](./orchestrator-prompt.md)頁面。 複製完整提示並從`split-payment-orchestrator/`目錄執行。

### 步驟3.2 — 安裝相依性並部署

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

部署後，`aio app deploy`會列印動作URL。 記下`demo-dashboard` URL — 您在階段4中需要它。

### 步驟3.3 — 確認事件註冊

在Adobe Developer Console中，開啟App Builder專案工作區。 在&#x200B;**事件**&#x200B;底下，確認`Split payment — sales order place before`註冊存在且已繫結至`payment-orchestrator`動作。

## 階段4 — 測試整個流程

依序執行驗證步驟。 [測試與驗證指南](./testing-and-verification.md)具有完整的curl命令、模擬指令碼使用方式，以及下列每個步驟的疑難排解參考。

### 步驟4.1 — 驗證REST端點是否可路由

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### 步驟4.2 — 測試簽出UI

1. 以測試客戶（擁有商店點數的客戶）身分登入店面
2. 將產品加入購物車，在運送和稅金後總計不到$100
3. 在付款步驟中，選取&#x200B;**現金**
4. 確認顯示的分割付款欄位：顯示餘額、預付現金、以$0儲存貸方
5. 減少現金金額，並確認正確更新商店信用部分

### 步驟4.3 — 測試臨界值保護

新增總金額超過$100的產品、進行結帳、選取「現金」，然後嘗試下訂單。 必須顯示錯誤`"Payment could not be processed. Please try again or contact support."`。

### 步驟4.4 — 發出測試分割付款單

建立低於$100的購物車，輸入現金/商店信用分割，然後下訂單。 在Commerce Admin中，確認：

* 訂單狀態為`pending_payment`
* 訂單上會顯示來自App Builder的兩個註解：
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

如果未出現App Builder註解，請先檢查`aio app logs`的記錄再繼續。

### 步驟4.5 — 透過模擬指令碼測試接受

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

在Commerce Admin中，確認：

* 訂單狀態已變更為`processing`
* 歷程記錄註解： `"Cash payment of $X.XX received."`
* 「商業發票」頁標上顯示的現金商業發票
* 「出貨」頁標上顯示的出貨（若適用）

### 步驟4.6 — 透過模擬指令碼測試拒絕

再下個測試訂單，然後：

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

確認訂單狀態為`canceled`且`split_cash_status`為`declined`。

### 步驟4.7 — 測試示範控制面板

從步驟3.2開啟控制面板URL。 系統中有暫止的訂單：

1. 確認訂單出現在擱置清單中
2. 按一下[接受]****，並確認在Commerce中將訂單移至`processing`
3. 下新訂單，返回儀表板，然後按一下&#x200B;**拒絕** — 確認取消

## 階段5 （選用） — 新增Experience Cloud UI擴充功能

此階段會將操作員儀表板內嵌到Commerce管理員殼層中，而不是使用獨立的`demo-dashboard`。 如果獨立儀表板符合您的需求，請略過此階段。

### 步驟5.1 — 檢閱先決條件

除了OAuth整合值之外，Experience Cloud UI擴充功能還需要IMS憑證。 在執行提示前，請先閱讀[Experience Cloud UI擴充功能AI提示](./experience-cloud-ui-prompt.md)頁面，並留意`OAUTH_CLIENT_ID`和相關IMS變數。

### 步驟5.2 — 執行UI擴充功能提示

將完整提示從&#x200B;**提示開始**&#x200B;複製到&#x200B;**提示結束**，並從`commerce-checkout-starter-kit/`目錄執行。

### 步驟5.3 — 部署

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## 快速疑難排解參考

| 症狀 | 首先要檢查的事項 |
|---|---|
| 分割付款欄位不會在結帳時顯示 | 瀏覽器主控台中有RequireJS錯誤；同時執行`bin/magento cache:flush` |
| `"Payment could not be processed"`已下訂單 | `PlaceOrderPlugin`或`BalanceManagementInterface` — 在`PlaceOrderPlugin`中新增暫存偵錯記錄 |
| 訂單上沒有App Builder註解 | 執行`aio app logs`以檢視是否引發事件以及動作傳回的專案 |
| App Builder中Commerce REST的`403` | Fastly IP允許清單 — 先使用模擬指令碼測試；如果這樣有效，問題是輸出IP封鎖 |
| 來自Commerce的`"The signature is invalid"` | 確認`COMMERCE_BASE_URL`沒有結尾斜線；確認整合已啟用 |
| 訂單未出現在示範儀表板中 | 檢查`extension_attributes.split_cash_status`是否出現在直接`GET /rest/V1/orders`回應中 |

如需完整的疑難排解詳細資訊，請參閱[測試與驗證指南](./testing-and-verification.md#common-issues-and-fixes)。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
