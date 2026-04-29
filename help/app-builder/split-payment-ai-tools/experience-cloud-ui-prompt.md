---
title: 分割付款POC：Experience Cloud UI擴充功能AI提示
description: 瞭解如何使用此選用提示在Commerce管理員中內嵌分割付款：管理員UI SDK、IMS、OAuth、接受和拒絕以及模擬指令碼。
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 629bbb6fe26f128e346d85c857111c2f8dbb6d76
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# 分割付款POC：Experience Cloud UI擴充功能AI提示

此選擇性步驟使用`commerce-checkout-starter-kit`和`commerce-backend-ui-1`模式，在&#x200B;**[!UICONTROL Adobe Commerce]**&#x200B;管理殼層(Experience Cloud)中嵌入分割付款訂單面板。 App Builder Orchestrator的獨立[示範儀表板](./orchestrator-prompt.md)涵蓋相同的接受和拒絕流程，而不需要管理員殼層整合。

## 如何使用這個提示

從&#x200B;**PROMPT START**&#x200B;到&#x200B;**End of prompt**&#x200B;的所有內容複製到Cursor或Claude中。 從`commerce-checkout-starter-kit/`目錄執行提示。

## 執行之前

* 此路徑除了OAuth值還需要&#x200B;**IMS**&#x200B;認證（請參閱`commerce-checkout-starter-kit`變數的[分割付款POC：環境變數參考](./env-reference.md)）。
* 完成[分割付款POC：如果您想要比較相同的`payment-accept`和`payment-decline`行為，請先完成App Builder orchestrator AI提示](./orchestrator-prompt.md)；UI擴充功能會以`COMMERCE_INTEGRATION_*`環境名稱重複使用該邏輯。


## 提示

**提示開始**


您正在為分割付款PoC產生`commerce-backend-ui-1`管理員UI SDK擴充功能。 此擴充功能會使用Adobe Commerce Checkout Starter Kit中的`@adobe/aio-app-dev-toolkit`和`@adobe/commerce-backend-ui-1`模式，將分割付款訂單控制面板內嵌到Commerce Admin Shell中。

**基礎專案：** `commerce-checkout-starter-kit/`
**延伸目錄：** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### 此擴充功能提供哪些功能

1. **管理員訂單格線面板** — Commerce管理員殼層中的自訂功能表專案，列出具有`split_cash_status = 'pending'`的分割付款訂單
2. **訂單詳細資料檢視** — 與訂單一起顯示分割付款明細（現金金額、商店貸方金額、狀態）
3. **接受/拒絕動作** — 透過OAuth 1.0a呼叫`payment-accept`和`payment-decline` App Builder動作的按鈕


### 要產生的檔案結構

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### 後端動作

**`actions/commerce/index.js`** — IMS驗證的Commerce REST
* 使用管理員UI SDK內容提供的IMS代號來呼叫Commerce REST
* 使用`split_cash_status`篩選器擷取訂單清單
* 以JSON傳回訂單清單

**`actions/payment-accept/commerce-client.js`** — OAuth 1.0a使用者端
* 與`split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`相同的實作
* 使用`COMMERCE_INTEGRATION_*`首碼式env var （以區分IMS認證）

**`actions/payment-accept/index.js`** — 接受動作
* 與`split-payment-orchestrator/actions/payment-accept/index.js`相同的邏輯
* 透過OAuth 1.0a呼叫`POST /V1/split-payment/orders/:orderId/cash-received`

**`actions/payment-decline/index.js`** — 拒絕動作
* 呼叫`POST /V1/split-payment/orders/:orderId/cash-decline`

**`actions/registration/index.js`** — 管理員UI SDK註冊
* 向Commerce管理員殼層註冊擴充功能
* 在分割付款儀表板的「訂單」下新增功能表專案


### React前端元件

**`SplitPaymentDashboard.jsx`**
* 在頻譜樣式的表格中列出暫緩分割付款訂單
* 欄位：訂單編號(increment_id)、日期、客戶、現金到期日、商店貸方、狀態
* 每列接受和拒絕按鈕
* 透過`web-src/src/utils.js`擷取協助程式呼叫後端動作
* 顯示載入/錯誤狀態；動作完成時重新整理

**`SplitPaymentOrderDetail.jsx`**
* 顯示單一訂單的分割付款明細
* 顯示：現金金額、商店貸方金額、目前split_cash_status

**`useSplitPaymentOrders.js`** — React勾點
* 從`actions/commerce/index.js`擷取分割付款訂單
* 傳回`{ orders, loading, error, refresh }`


### 模擬指令碼

**`scripts/simulate-split-payment.mjs`**

Node.js ESM指令碼，可直接測試Commerce REST呼叫（不透過App Builder）。 使用與App Builder動作相同的OAuth 1.0a簽署。

命令：
* `node simulate-split-payment.mjs help` — 顯示使用狀況
* `node simulate-split-payment.mjs list` — 列出具有分割付款資料的最近訂單
* `node simulate-split-payment.mjs show <orderId>` — 顯示特定訂單(entity_id)的分割付款欄位
* `node simulate-split-payment.mjs accept <orderId>` — 呼叫`cash-received`端點
* `node simulate-split-payment.mjs decline <orderId>` — 呼叫`cash-decline`端點

從`commerce-backend-ui-1/.env.simulation`讀取認證（從`.env.simulation.example`複製）。

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

使用下列專案設定擴充功能：
* `commerce-backend-ui-1`延伸型別
* 四個後端動作(`commerce`、`payment-accept`、`payment-decline`、`registration`)
* `require-adobe-auth: true`用於所有動作，但`registration`除外
* 來自`COMMERCE_INTEGRATION_*`認證之環境的輸入繫結


### 產生檔案之後

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### 提示結尾


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
