---
title: 分割付款POC：先決條件與環境設定
description: 瞭解如何在分割付款建置提示之前設定Commerce、COD和商店點數管理員、OAuth整合、I/O事件、App Builder和aio CLI。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: d5f1e76c3a5127698f2933810fca218b79082571
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# 分割付款POC：先決條件與環境設定

在執行任何建置提示之前，請先完成本教學課程的每個步驟。 缺少單一步驟是流程在教學課程中中斷的最常見原因。

## &#x200B;1. Adobe Commerce需求

* Adobe Commerce **2.4.5或更新版本** （內部部署或Commerce Cloud）
* Git存取Commerce專案（您在`app/code/`下新增模組）
* 存取&#x200B;**[!UICONTROL Commerce Admin]**

### 僅限Commerce Cloud：啟用I/O事件

新增下列專案至`.magento.env.yaml`並部署，然後再新增模組：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **警告：**&#x200B;必須順利完成此部署，I/O事件模組相依性才能解析。


## &#x200B;2. Commerce管理設定

請先執行這些步驟，然後再執行其他作業。 結帳JavaScript取決於完全相符的字串。

### 2a。 使用確切標題啟用「貨到付款」

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**： **是**
* **[!UICONTROL Title]**： **`Cash`** （此確切字串是結帳JavaScript符合的專案）

> **備註：**&#x200B;若您的商店使用其他貨到付款(COD)實作或標題，請在Commerce模組中調整`payment-method-helper.js`。

### 2b. 啟用商店點數

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**： **是**

### 2c。 新增商店點數至您的測試客戶

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[您的測試客戶]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

在商店點數中新增至少&#x200B;**$50**。 您將以總金額低於$100的訂單進行測試。

### 2d。 建立Commerce整合

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**： `Split Payment App Builder` （或您偏好的任何名稱）
* 在&#x200B;**[!UICONTROL API]**&#x200B;索引標籤上，至少授予：
   * `Magento_Sales::actions` （`cash-received`端點必要）
   * `Magento_Sales::cancel` （`cash-decline`端點必要）
   * 報價或購物車管理（完整或相關的子集）
   * **[!UICONTROL Customer balance]** （完整或相關的子集）

選取&#x200B;**[!UICONTROL Save]**，然後選取&#x200B;**[!UICONTROL Activate]**。

**複製全部四個值；它們只會顯示一次：**

| 值 | 環境變數 |
| --- | --- |
| 使用者金鑰 | `COMMERCE_CONSUMER_KEY` |
| 使用者密碼 | `COMMERCE_CONSUMER_SECRET` |
| 存取權杖 | `COMMERCE_ACCESS_TOKEN` |
| 存取權杖密碼 | `COMMERCE_ACCESS_TOKEN_SECRET` |

安全地儲存這些值。 您在每個App Builder `.env`檔案中都需要這些檔案。


## &#x200B;3. Adobe Developer Console和App Builder

* 存取Adobe Developer Console組織
* **App Builder專案** （新的或您重複使用的專案）
* 已設定工作區；提示假設&#x200B;**[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]**&#x200B;已新增為工作區中的服務
* **Commerce**&#x200B;已連線為工作區的事件提供者

概念證明中使用的事件代碼是： `com.adobe.commerce.observer.sales_order_place_before`

如果您尚未以事件提供者的身分連線Commerce，請參閱Commerce擴充性指南中的[設定Commerce以向Adobe I/O發出事件](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"}。


## &#x200B;4. 本機開發環境

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. 需要瞭解的兩個專案目錄

本教學課程使用兩個不同的目錄根目錄。 將它們分開。

**Commerce專案根目錄** （您的Magento Git存放庫）：

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder專案根目錄** （來源封裝的`split-payment-orchestrator`資料夾，或您建立的新專案）：

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id與increment_id比較

> **在REST呼叫中一律使用`entity_id` （數值資料庫識別碼），而非`increment_id` （例如`000000042`）。**

從&#x200B;**[!UICONTROL Commerce Admin]**&#x200B;訂單URL尋找`entity_id`：

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

或從模擬指令碼：

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. 100美元的臨界值

分割付款UI與臨界值保護目標訂單&#x200B;**$100.00或以下**。 流程的行為如下：

* **在$100或以下：**&#x200B;出現分割付款UI；客戶可以設定現金加商店信用分割
* **超過$100：** `CheckoutPlugin`在付款步驟擲回錯誤

若要測試，請建立小計、運費和稅金為&#x200B;**小於或等於$100**&#x200B;的購物車（例如，低於$90的產品，因此運費和稅金仍可低於上限）。

臨界值會儲存在：

* Commerce設定： `split_payment/general/threshold` （在`etc/config.xml`中的預設`100`）
* App Builder環境： `PAYMENT_THRESHOLD=100` （必須符合Commerce）


## &#x200B;8. Fastly （僅限Commerce Cloud）

App Builder動作會透過REST (`/rest/V1/split-payment/orders/...`)呼叫Commerce。 如果您的Commerce Cloud專案使用Fastly搭配IP允許清單，App Builder執行階段輸出IP位址必須加入允許清單。

**如何檢查：**&#x200B;請先執行模擬指令碼（直接使用OAuth簽署進行curl）。 如果這樣有效，但App Builder動作傳回`403`，Fastly可能會封鎖要求。

使用Adobe目前的App Builder輸出IP範圍檔案，並視需要將其新增到您的Fastly設定。


## 驗證檢查清單

開始建置提示之前，請確認下列專案：

* [ ] Commerce版本為2.4.5或更新版本
* [ ] I/O事件已啟用(Commerce Cloud)並已部署
* [ 「]貨到付款」已啟用，其標題設定為`Cash`
* [ ]個商店點數已啟用
* [ ]測試客戶至少有$50美元的商店點數
* [ ] Commerce整合已建立並啟動，而且所有四個OAuth值都已儲存
* [ ] App Builder專案已設定I/O事件服務和Commerce事件提供者
* [ ] `aio login`已完成，且已透過`aio app use`選取正確的工作區
* [ 已安裝] Node.js 18或更新版本，且已安裝`aio` CLI
* [ ] `.env`檔案是根據[分割付款POC：環境變數參考](./env-reference.md) （以及您的來源封裝，如果您使用一個）


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
