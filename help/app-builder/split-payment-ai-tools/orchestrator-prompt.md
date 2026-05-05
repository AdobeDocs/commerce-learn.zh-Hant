---
title: 分割付款POC：App Builder orchestrator AI提示
description: 瞭解如何使用此提示來建置分割付款協調器應用程式。 I/O事件、支付協調器、網頁動作、示範儀表板，以及aio應用程式部署。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# 分割付款POC：App Builder orchestrator AI提示

使用此頁面來複製產生&#x200B;**split-payment-orchestrator**&#x200B;專案的完整提示：**payment-orchestrator** I/O事件消費者、**payment-accept**&#x200B;和&#x200B;**payment-decline**&#x200B;網頁動作、**demo-dashboard**&#x200B;以及Commerce REST使用者端。

## 如何使用這個提示

從&#x200B;**PROMPT START**&#x200B;到&#x200B;**提示**&#x200B;的結尾的所有內容都複製到游標（使用Claude）或直接複製到Claude。 從`split-payment-orchestrator/`目錄（App Builder專案根目錄）執行提示。

## 執行之前

* 完成[分割付款POC：必要條件和環境設定](./prerequisites-and-setup.md)。
* 在專案中準備好您的[分割付款POC：環境變數參考](./env-reference.md)和`.env`檔案。


## 提示

**提示開始**


您正在產生完整的Adobe App Builder應用程式，以便在Adobe Commerce中協調分割付款。 此應用程式會接收來自Commerce的I/O事件、處理分割付款決策，以及透過REST回撥至Commerce。

**專案：** `split-payment-orchestrator`
**執行階段：** Node.js 18
**金鑰相依性：** `@adobe/aio-sdk ^6.0.0`，`got ^11.8.6`，`oauth-1.0a ^2.2.6`

產生下列所有檔案。 應用程式必須搭配Adobe I/O Runtime (`aio app deploy`)使用。


### 要產生的檔案結構

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

定義`split_payment_orchestrator`封裝中的四個動作：

**`payment-orchestrator`**
* `web: "no"` （僅限I/O事件觸發程式 — 無法透過HTTP直接呼叫）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 來自環境的輸入： `LOG_LEVEL`，`COMMERCE_BASE_URL`，`COMMERCE_CONSUMER_KEY`，`COMMERCE_CONSUMER_SECRET`，`COMMERCE_ACCESS_TOKEN`，`COMMERCE_ACCESS_TOKEN_SECRET`，`PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` （HTTP Web動作 — 可由控制面板或ERP呼叫）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 相同的Commerce認證輸入（無`PAYMENT_THRESHOLD`）

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 輸入相同的Commerce認證資料

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` ←儀表板可公開存取（若有設定，僅受`DEMO_UI_SECRET`保護）
* 輸入：所有Commerce認證+ `DEMO_UI_SECRET`， `DEMO_UI_BASE_URL`

**事件註冊** （在`events.registrations`下）：

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

共用Adobe Commerce的OAuth 1.0a REST使用者端。 實作：

**`createCommerceClient(params, logger)`** — 傳回`{ request, baseUrl }`

* 從`params`讀取`COMMERCE_BASE_URL`、`COMMERCE_CONSUMER_KEY`、`COMMERCE_CONSUMER_SECRET`、`COMMERCE_ACCESS_TOKEN`、`COMMERCE_ACCESS_TOKEN_SECRET`
* 遺失任何認證時擲回
* 使用`oauth-1.0a`搭配`HMAC-SHA256` (Node.js `crypto.createHmac`)
* 使用`got@11` （不是`got@12+` — 專案使用CJS）搭配`prefixUrl = ${baseUrl}/rest/V1/`
* 透過`beforeRequest`連結新增`Authorization`標題
* `request(method, path, options)` — 標準化路徑（前導`/`的移除），傳回`{ statusCode, body }`
* `throwHttpErrors: false` — 絕不會在4xx/5xx上擲回；一律會傳回狀態代碼


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — 傳回`{ pass: boolean, reason: string }`

邏輯：
1. 讀取`params.PAYMENT_THRESHOLD`；剖析為浮點數；如果遺失、NaN或≤0，則預設為`100`
2. 若`orderTotal > threshold`：傳回`{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. 若`Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`：傳回`{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. 否則：傳回`{ pass: true, reason: '' }`


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — 傳回`{ ok: boolean, error? }`

* 以下列方式呼叫`POST orders/${orderId}/comments`：

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* 在2xx上傳回`{ ok: true }`；否則傳回`{ ok: false, error: { code, message } }`
* 在try/catch中換行；傳回錯誤物件，不會擲回


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — 傳回`{ ok: boolean, error? }`

* 若`success`：張貼歷程記錄註解`"Split payment orchestration completed. Order awaiting cash confirmation."`
* 若`!success`：貼文`"Payment could not be processed. Please try again or contact support."` — 絕對不要在客戶可見的註解中包含`detail`；僅於內部記錄`detail`
* 傳回`{ ok: boolean, error? }` — 永不擲回


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** — 已棄用的no-op實作

包含此檔案並附上JSDoc `@deprecated`通知，說明：
> Orchestrator不再透過REST套用商店點數。 使用`BalanceManagementInterface::apply()`在Commerce PHP模組(`PlaceOrderPlugin`)中結帳時套用商店點數，這需要使用中的購物車。 等到App Builder收到I/O事件時，購物車即已停用。 此檔案會保留以供參考或供自訂流程使用，其中儲存點數會在訂購後套用。

維持有效的實作（與其他模組相同的形狀），讓開發人員可以研究模式，但請將其清楚標示為未用於目前流量。


### `actions/payment-orchestrator/index.js`

I/O事件進入點。 實作`async function main(params)`。

**承載擷取：**

Adobe Commerce I/O事件可能會以數個圖形傳送裝載。 透過按順序檢查這些路徑來擷取order值物件：
1. `params.__ow_body` （視需要剖析JSON字串） → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. 回覆`params`本身

**欄位擷取自值：**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* 從`value.extension_attributes`分割金額（檢查`extension_attributes`和`extensionAttributes`）：
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**流量：**
1. 擷取值；如果缺少`orderId`，則記錄錯誤並傳回`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. 呼叫`evaluateThreshold(...)` — 如果失敗，則記錄並傳回相同的200回應
3. 呼叫`createCommerceClient(params, logger)` — 如果失敗，則傳回200錯誤
4. 如果`storeCredit > 0`，則記錄該商店點數已在結帳時套用（不需要REST呼叫）
5. 呼叫`recordCashPending(...)` — 如果失敗，則呼叫`updateOrderAfterOrchestration(..., success: false)`並傳回200錯誤
6. 撥打`updateOrderAfterOrchestration(..., success: true)`
7. 傳回`{ statusCode: 200, body: { ok: true, message: 'processed' } }`

**重要：**&#x200B;一律傳回`statusCode: 200` — I/O執行階段將重試非200個回應，這會導致重複訂單處理。 錯誤會在內文中報告。

**`PUBLIC_ERROR`常數：** `"Payment could not be processed. Please try again or contact support."` — 用於所有對外錯誤訊息。


### `actions/payment-accept/index.js`

HTTP Web動作。 呼叫`POST /V1/split-payment/orders/:orderId/cash-received`。

**訂單ID解析度：**&#x200B;檢查`params.orderId`，然後檢查`params.payload.orderId`，再檢查`params.__ow_body` （已剖析的JSON）。 如果遺失，則傳回400。

**流量：**
1. 解析`orderId`；如果遺失，則傳回400
2. Init商務使用者端；如果失敗，則傳回500
3. 使用空白JSON內文呼叫`POST split-payment/orders/${orderId}/cash-received`
4. 如果2xx：傳回`{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`
5. If error：記錄並傳回`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

HTTP Web動作。 與`payment-accept`相同的模式，但呼叫`POST /V1/split-payment/orders/:orderId/cash-decline`。

成功時傳回`{ ok: true, orderId, message: 'declined' }`。


### `actions/demo-dashboard/index.js`

自含的示範操作員儀表板。 提供HTML儀表板，以列出暫緩現金訂單並觸發接受/拒絕動作。 這是同時提供HTML UI和JSON API的單一Web動作。

**安全性：**
* 選擇性`DEMO_UI_SECRET`檢查：如果已設定，則在所有要求上需要`?secret=<value>`查詢引數或`x-demo-secret`標頭。 若遺失/錯誤，則傳回401。
* 如果未設定`DEMO_UI_SECRET` （儀表板未受保護），則記錄警告

**路由（根據HTTP方法+路徑/內文）：**

動作會接收所有請求。 決定意圖來源：
* `params.__ow_method` (GET/POST)和`params.__ow_path`或動作引數
* `GET`沒有動作→提供HTML儀表板
* 具有`action=list`引數的`GET`→傳回擱置訂單的JSON清單
* 具有`action=accept`和`orderId`的`POST`→呼叫`payment-accept`邏輯（或內嵌Commerce REST呼叫）
* 具有`action=decline`和`orderId`的`POST`→呼叫`payment-decline`邏輯

**正在擷取擱置中的訂單：**
* 從Commerce REST呼叫`GET orders?searchCriteria[pageSize]=50`
* 篩選到`extension_attributes.split_cash_status === 'pending'` AND訂單未處於終端機狀態的訂單(`complete`， `closed`， `canceled`， `cancelled`)
* 在記憶體中排序最新優先
* 最多傳回20 （可透過`limit`引數設定）

**HTML儀表板：**
儀表板是隨`Content-Type: text/html`傳回的HTML字串。 它應該：
* 在表格中列出暫緩現金訂單：訂單編號(increment_id)、entity_id、客戶名稱、現金金額、商店貸方金額、日期
* 針對呼叫動作本身API端點的每個訂單，提供&#x200B;**接受**&#x200B;和&#x200B;**拒絕**&#x200B;按鈕
* 顯示內嵌的成功/錯誤回應
* 包含重新整理按鈕
* 已設定足夠樣式以可使用（最小CSS沒有問題；沒有外部CDN相依性可避免Runtime中的CORS問題）
* 如果未設定`DEMO_UI_SECRET`，則顯示警告橫幅

**在UI中顯示錯誤：**
當Commerce REST呼叫失敗時，請包含HTTP狀態和Commerce錯誤本文內容的簡短說明（整理 — 移除HTML標籤，截斷為500個字元）。 不要公開認證。

**回應協助程式：**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### 部署命令

產生所有檔案後，從`split-payment-orchestrator/`目錄：

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

部署後，記下`aio app deploy`列印的動作URL。 `demo-dashboard` URL是您存取操作員控制面板的位置。


### 提示結尾


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
