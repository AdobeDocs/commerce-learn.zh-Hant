---
title: 分割付款POC — 測試與驗證指南
description: 瞭解如何驗證分割付款POC。 Commerce安裝、REST、結帳、臨界值、模擬接受和拒絕、示範儀表板，以及App Builder記錄。
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# 分割付款POC：測試與驗證指南

本指南會逐步引導您驗證每個元件是否正確運作，其測試順序與應測試的順序相同。 從下而上開始（Commerce模組），然後向上運作(App Builder)。


## 步驟1 — 確認Commerce模組安裝

執行`bin/magento setup:upgrade`和`bin/magento setup:di:compile`之後：

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

預期輸出：在`sales_order`中顯示以`split_`開頭的四個資料行。


## 步驟2 — 驗證Commerce管理設定

在Commerce Admin中：
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — 確認&#x200B;**[!UICONTROL Cash On Delivery]**&#x200B;已啟用且標題為`Cash`
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — 確認啟用
3. 確認您的測試客戶擁有商店點數： **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[客戶]* > **[!UICONTROL Store Credit]**


## 步驟3 — 驗證REST端點是否可供存取

使用curl確認端點回應（它們會拒絕未授權請求，但401會確認它們已正確路由）：

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## 步驟4 — 測試簽出UI

1. 以測試客戶（擁有商店點數）身分登入店面
2. 新增產品至購物車（運費加稅後總計低於$100）
3. 繼續結帳
4. 在付款步驟中，選取&#x200B;**現金** （或貨到付款）
5. 確認分割付款欄位是否顯示：
   * 顯示的商店貸方餘額
   * 預填訂單總計的現金金額欄位
   * 顯示$0.00的「此訂單的商店貸方」欄位（因為現金=整張訂單總計）
6. 減少現金金額（例如，為$50的訂單輸入$10）
7. 確認商店信用部分更新為$40.00
8. 確認訊息是否顯示： `"The remaining $40.00 will automatically be applied from your store credit."`

**測試驗證：**
* 輸入大於訂單總計→錯誤訊息的現金金額
* 輸入需要比錯誤訊息更多商店信用額→現金金額
* 輸入現金= 0→錯誤（或商店信用涵蓋整個訂單）


## 步驟5 — 測試臨界值保護

1. 新增總計超過$100的產品（小計+運費+稅金> $100）
2. 繼續結帳，選取&#x200B;**現金**
3. 嘗試下訂單
4. 驗證是否顯示錯誤訊息： `"Payment could not be processed. Please try again or contact support."`
5. 驗證購物車是否已保留（客戶仍然可以調整購物車並重試）


## 步驟6 — 發出測試分割付款單

1. 建立低於$100的購物車（使用商店點數登入的客戶）
2. 選取結帳時現金
3. 輸入小於訂單總計的現金金額（例如$45訂單中的$10）
4. 確認顯示商店信用訊息
5. 按一下&#x200B;**下單**

下單後，在Commerce管理員中確認：
* 訂單處於`pending_payment`狀態
* Order有兩個歷程記錄註解：
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` （來自App Builder `payment-orchestrator`）
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` （來自App Builder）
* 分割付款金額會顯示在訂單付款區塊中

> **如果未出現App Builder註解：**&#x200B;請檢查App Builder動作記錄檔與`aio app logs`。 事件可能尚未引發，或動作可能有錯誤。


## 步驟7 — 透過模擬指令碼測試接受

模擬指令碼是在沒有完整運運算元UI的情況下測試接受/拒絕流程的最快方式。

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

接受後，在Commerce管理員訂單檢視中確認：
* 訂單狀態為`processing`
* 歷程記錄註解： `"Cash payment of $X.XX received."`
* 已建立現金商業發票（顯示在「商業發票」頁標中）
* 已建立出貨（若適用，顯示在「出貨」頁標中）
* 歷程記錄註解： `"Split payment: cash portion invoiced #XXXXXXXX."`
* 歷程記錄註解： `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## 步驟8 — 透過模擬指令碼測試拒絕

下另一個測試訂單（與步驟6的設定相同），然後：

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

拒絕後，請在Commerce管理員中確認：
* 訂單狀態為`canceled`
* 歷程記錄註解： `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## 步驟9 — 測試示範控制面板

部署`split-payment-orchestrator`後，`aio app deploy`會列印動作URL。

在瀏覽器中開啟`demo-dashboard` URL：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

若已設定`DEMO_UI_SECRET`：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

含待定訂單：
1. 儀表板應該顯示擱置清單中的順序
2. 按一下[接受]&#x200B;**&#x200B;**→順序應移至Commerce中的`processing`
3. 下其他訂單；按一下Commerce中的&#x200B;**拒絕**→訂單應為`canceled`


## 步驟10 — 測試App Builder動作記錄

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

若為`payment-orchestrator`，請尋找：

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

針對`payment-accept`或`payment-decline`：

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## 常見問題與修正

### Commerce OAuth中的「簽名無效」

**原因：** App Builder執行階段和Commerce之間的時鐘扭曲，或OAuth簽署錯誤。

**修正：**
* 確認`COMMERCE_BASE_URL`沒有結尾斜線
* 確認四個OAuth認證用於&#x200B;_已啟動_&#x200B;整合
* 先使用模擬指令碼測試 — 如果指令碼正常運作，但App Builder無法運作，則可能是未載入環境變數（請檢查`aio app deploy`輸出是否有遺漏的環境變數）

### 分割付款欄位未出現在結帳中

**原因：** LayoutProcessorPlugin插入路徑與您的簽出配置不符。

**修正：**
* 檢查瀏覽器主控台是否有載入`Client_SplitPayment/js/view/payment/split-payment`的RequireJS錯誤
* 檢查`bin/magento setup:static-content:deploy`已成功完成
* 排清快取： `bin/magento cache:flush`
* 如果您的主題有自訂簽出，則插入元件的`LayoutProcessorPlugin`路徑可能需要調整

### 商店信用未套用/「無法處理付款」的訂貨單

**原因：**&#x200B;通常是其中一個邊緣案例外掛程式無法正常運作。

**檢查：**
1. `SplitPaymentSession`是否傳回正確的金額？ 在`PlaceOrderPlugin`中新增暫存偵錯記錄
2. `FixSplitPaymentGrandTotalPlugin`是否在呼叫`BalanceManagementInterface::apply()`之前執行並影響報價總計？ `beginBalanceApply()`旗標應該會隱藏它。
3. Commerce中是否已啟用客戶平衡模組？

### App Builder動作會接收事件，但缺少orderId

**原因：** `io_events.xml`欄位清單不包含`entity_id`，或事件裝載圖形已變更。

**修正：**
* 確認`io_events.xml`在欄位清單中包含`entity_id`
* 在動作中，暫時記錄`JSON.stringify(params)`以檢視完整的裝載圖形
* 檢查`extractValue()`函式是否找到正確的巢狀層級

### 訂單未顯示在示範儀表板中

**原因：** Commerce REST `orders`搜尋條件未傳回訂單，或`split_cash_status`欄位不在REST回應中。

**修正：**
* 確認`OrderRepositoryPlugin`是否正確載入擴充功能屬性
* 直接測試： `GET /rest/V1/orders?searchCriteria[pageSize]=5`並檢查回應中是否顯示`extension_attributes.split_cash_status`
* 檢查`extension_attributes.xml`是否正確宣告`OrderInterface`上的`split_cash_status`屬性


## 驗證檢查清單

* [ ] `split_*`欄顯示在`sales_order`資料表中
* [ 在沒有驗證的情況下呼叫時，]個REST端點會傳回401 （非404）
* [ 選取「現金」時，]分割付款UI會在結帳時轉譯
* [ ]驗證訊息有效（超額付款、信用額度不足）
* [ ]臨界值防護封鎖訂單> $100
* [ ]已下訂單有`pending_payment`個狀態和App Builder註解
* [ ] `simulate-split-payment.mjs list`顯示含有分割金額的測試訂單
* [ ] `simulate-split-payment.mjs accept <id>`將訂單移至`processing`，並附上發票與出貨
* [ ] `simulate-split-payment.mjs decline <id>`取消訂單
* [ ]示範儀表板會列出擱置的訂單，並接受/拒絕來自UI的工作


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
