---
title: Adobe Commerce Developer Agent App Builder練習
description: 透過此實作Commerce練習，瞭解如何使用Adobe Commerce Developer Agent建置、部署和測試三個App Builder擴充性使用案例。
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 92af5355fa31c1ce9e627679b0a1bb92cce0e1d8
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---

# Adobe Commerce Developer Agent App Builder練習

使用Commerce Developer Agent (CDA)建置、部署和測試Adobe Commerce擴充性使用案例的實作逐步解說。 此練習涵蓋三個使用案例：購物車數量限制webhook、高價值訂單保留，以及從Blueprint到功能測試的持有訂單事件導向封存。

## 快速入門

### 如何報告問題和回饋

在整個練習過程中，您會遇到粗邊 — 使用新特徵時預期會出現此情況。 使用上線期間提供的意見反應範本，擷取並與您的Adobe計畫聯絡人分享任何問題。

>[!TIP]
>
> 報告問題時：
>
> * 包含`projectId` （顯示在您的瀏覽器URL中）。
> * 在相關時加入熒幕擷取畫面。

### 先決條件

**帳戶和存取權**

* 至少&#x200B;**開發人員**&#x200B;角色（位於您的搶先存取IMS組織中）。
* 管理員存取該組織內的Adobe Commerce as a Cloud Service (ACCS)執行個體，請前往&#x200B;**Cloud Service執行個體**&#x200B;下方的experience.adobe.com。
* github帳戶。

**工具**

功能驗證需要Edge Delivery Services (EDS)店面。 您將需要：

* Node.js 22+
* Adobe I/O CLI： `npm install -g @adobe/aio-cli`
* AIO CLI Commerce外掛程式： `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

將店面樣板安裝在空的資料夾中，在出現提示時選取您的ACCS例項：

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

啟動店面：

```bash
cd storefront
npm run start
```

## 開啟Commerce Developer Agent

1. 導覽至Commerce Developer Agent，網址為experience.adobe.com，在&#x200B;**Developer Agent**&#x200B;下。
1. 使用您的搶先使用IMS組織憑證登入。

## 使用案例1：購物車最大單位webhook

此使用案例會使用同步Commerce webhook，在新增產品之前驗證購物車數量限制。

### Blueprint階段

輸入以下提示並按一下&#x200B;**產生Blueprint**：

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> 尋找下列專案：
>
> * 擷取需求的藍圖(v1)隨即建立。
> * 會建立任務以引導實施。

在聊天方塊中輸入詳細資訊，或按一下聊天方塊上方的Pills之一，以精進藍圖（*挑戰假設*、*尋找設計差距*&#x200B;等）。 在您滿意後，請按一下[核准計畫] **&#x200B;**&#x200B;以繼續進行。

### 開發階段

代理程式會轉換到「開發」階段，並開始布建工作區。

>[!NOTE]
>
> 在Explorer面板中尋找這些檔案：
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

布建後，代理程式會顯示實施作業清單，並開始建置。

>[!NOTE]
>
> 尋找下列專案：
>
> * 產生的程式碼符合要求。
> * `Validate`串流畫面顯示工作區驗證進度(`aio app build`)。
> * 如果驗證失敗，代理程式會自行更正產生的程式碼。

對程式碼感到滿意後，請按一下&#x200B;**整合**&#x200B;索引標籤以繼續進行。

### 設定整合

**連線或建立App Builder工作區**

若要建立或連線App Builder專案，請依照熒幕上的指示進行。

如果連線到現有工作區，請確定它具有：

* `Runtime`服務已新增。
* 已新增下列API： Adobe Commerce as a Cloud Service、I/O管理API、App Builder資料服務、I/O事件、Adobe Commerce適用的Adobe I/O Events。

如果建立新工作區，請手動新增&#x200B;**Adobe Commerce as a Cloud Service** API。

>[!IMPORTANT]
>
> 連線到現有的App Builder專案後，展開&#x200B;**進階設定**&#x200B;並貼上工作區JSON，然後按一下&#x200B;**重新檢查狀態**&#x200B;以確認已安裝所有必要的API。

按一下[下一步]&#x200B;**&#x200B;**&#x200B;繼續。

**連線到Commerce**

從清單中選取您的ACCS執行個體，或在&#x200B;**Commerce REST基底URL**&#x200B;欄位中輸入URL，然後按一下&#x200B;**連線Commerce執行個體**。 按一下[下一步]&#x200B;**&#x200B;**&#x200B;繼續。

**連線到GitHub**

輸入存放庫URL並使用GitHub應用程式或個人存取權杖，將工作區連線至GitHub存放庫。 按一下[下一步]&#x200B;**&#x200B;**&#x200B;繼續。

**設定環境變數**

填寫專案所需的任何環境變數。

### 部署

按一下&#x200B;**開發**&#x200B;以返回開發階段，然後要求代理程式在提示欄位中部署。

>[!NOTE]
>
> 尋找「確認部署」訊息，顯示「組織」、「專案」、「Workspace」和「執行階段」名稱空間。

確認部署。

>[!NOTE]
>
> 尋找：
>
> * 顯示預先部署驗證進度的`Validate`串流畫面。
> * 如果驗證失敗，代理程式會自行更正代碼。
> * 顯示部署進度(`aio app deploy`)的`Deploy`串流畫面。
> * 如果部署失敗，代理程式會自行更正程式碼。

### 在應用程式管理中建立應用程式的關聯

1. 導覽至您的ACCS執行個體管理員URL並登入。
1. 在左側功能表上選取&#x200B;**應用程式**，然後選取&#x200B;**應用程式管理**。
1. 按一下&#x200B;**+關聯應用程式** （右上方）。
1. 選取CDA部署到的專案和Workspace，然後按一下[關聯]。**&#x200B;**

>[!NOTE]
>
> 尋找顯示應用程式名稱和版本以及已實作功能（業務設定、Webhook、事件等）的卡片。

### 在App Management中安裝和設定

1. 在應用程式的資料列上，按一下&#x200B;**安裝**，然後按&#x200B;**關閉**。
1. 在同一列，按一下&#x200B;**設定**&#x200B;以填入企業設定值，然後按&#x200B;**關閉**。

>[!NOTE]
>
> 尋找顯示Blueprint所指定每個設定欄位（已預先填入您指定的預設值）的表單。

### 功能測試

1. 在App Management應用程式設定中，將&#x200B;**購物車單位上限**&#x200B;設定為3 （快速測試的低值）。
1. 在店面，從空的購物車開始。
1. 從產品詳細資料頁面(PDP)新增產品，直到總數量超過3為止 — 最後一次新增會失敗。
1. 在PDP上，您看到： *「您已達到專案數量上限。」*
1. 低於限制，仍可成功新增。

>[!NOTE]
>
> 從產品清單頁面(PLP)，封鎖的新增無訊息地失敗，沒有訊息 — 這是店面行為，不是webhook失敗。 偏好使用PDP進行驗證。

## 使用案例2：高價值訂單保留與驗證代碼

導覽回&#x200B;**Blueprint**&#x200B;階段以開始此使用案例。

### Blueprint階段

輸入以下提示並按一下&#x200B;**產生Blueprint**：

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> 尋找下列專案：
>
> * 擷取需求的藍圖(v2)隨即建立。
> * 原始計畫任務會保留。
> * 已新增與新需求對應的新任務。

視需要調整Blueprint，然後按一下&#x200B;**核准計畫**&#x200B;以繼續進行。

### 開發、部署、關聯及安裝

遵循使用案例1中使用的相同程式，從需求移至已安裝的應用程式 — 無需重新設定整合。

>[!IMPORTANT]
>
> 若要取得已關聯應用程式的變更，您必須在「應用程式管理」中再次&#x200B;**取消關聯**&#x200B;與&#x200B;**關聯**。

### 功能測試

1. 在App Management應用程式設定中，將&#x200B;**訂單保留閾值(USD)**&#x200B;設定為50 （在測試購物車中容易超過）。
1. 確認訂單自訂屬性存在（預設`lab_verification_code`）。
1. 下單的總金額超過$50的訂單。
1. 等待約30秒（事件為非同步；非優先順序傳送最多可能需要59秒）。
1. 在Commerce Admin → Sales → Orders中，開啟訂單。 狀態為&#x200B;**保留** (`holded`)；自訂屬性包含具有隨機值的`lab_verification_code`。
1. 選用性：將低於$50的訂單放在最前 — 此處理常式不會將其擱置。

## 使用案例3：事件導向的持有訂單封存

導覽回&#x200B;**Blueprint**&#x200B;階段以開始此使用案例。

### Blueprint階段

輸入以下提示並按一下&#x200B;**產生Blueprint**：

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> 尋找下列專案：
>
> * 擷取需求的藍圖(v3)隨即建立。
> * 原始計畫任務會保留。
> * 已新增與新需求對應的新任務。

視需要調整Blueprint，然後按一下&#x200B;**核准計畫**&#x200B;以繼續進行。

### 開發、部署、關聯及安裝

按照先前使用案例中使用的相同流程，從需求移至已安裝的應用程式 — 無需重新設定整合。

>[!IMPORTANT]
>
> 若要取得已關聯應用程式的變更，您必須在「應用程式管理」中再次&#x200B;**取消關聯**&#x200B;與&#x200B;**關聯**。

### 功能測試

1. 確認使用案例2臨界值夠低，無法進行測試（例如，應用程式設定為$50）。
1. 將訂單置於該臨界值之上，讓使用案例2將其保留（~30秒）。
1. 在Adobe Developer Console →您的專案→預備→動事件中，開啟保留訂單封存事件的註冊（在安裝時新增或更新）。
1. 確認在訂單移至保留狀態後，事件已遞送至該註冊。 使用連結至`order-archive/archive-held-order`之Commerce事件的事件追蹤或監視。

>[!NOTE]
>
> 事件為非同步 — 在訂單被保留後，最多可允許約30至59秒。

## 疑難排解

如果CDA產生的應用程式未如預期執行或產生錯誤，請要求代理程式從開發階段進行疑難排解。

>[!NOTE]
>
> CDA無法檢視外部發生的步驟。 關聯、安裝、設定和功能測試都在Commerce管理、應用程式管理或店面中執行，而不是在CDA中執行。 如果問題在其中某個區域出現，代理程式將無法看到問題發生，因此請提供缺少的內容：
>
> * 您所做的事以及在哪裡（例如「按一下[在應用程式管理中安裝]」）。
> * 您期望發生的情況。
> * 發生什麼事。
> * 熒幕上顯示的確切錯誤文字或訊息。
> * 來自瀏覽器主控台或Adobe Developer Console的App Builder記錄檔和事件註冊偵錯追蹤的任何相關錯誤。

報告越具體，代理程式診斷問題的能力就越好。

## 可選步驟

**下載代碼**

若要繼續縮小或編輯您最喜愛的IDE，請按一下「開發階段總管」工具列上的「下載」圖示來下載CDA產生的程式碼。 選取目的地資料夾，按一下[儲存] **&#x200B;**，然後解壓縮工作區封裝。

>[!NOTE]
>
> 尋找：
>
> * 「開發階段總管」中顯示的所有檔案都會出現在解壓縮的資料夾中。
> * 使用`aio app build`建置專案時沒有「編譯」錯誤。

若要使用CDA使用的相同代理程式技能，請將其安裝在您的專案資料夾中：

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

然後啟動IDE或CLI並開始提示。

**透過檔案或連結附加內容**

您可以使用文字檔或連結來附加前後關聯，而不直接在Blueprint或「開發」階段中提示：

1. 按一下聊天方塊上的附件圖示。
1. 按一下&#x200B;**新增檔案**&#x200B;上傳本機文字檔，或輸入URL並按一下&#x200B;**新增連結**&#x200B;透過遠端檔案新增內容。
1. 按一下&#x200B;**完成**，然後輸入提示以輕推代理程式。

>[!NOTE]
>
> 尋找將附件中的內容合併到下一輪的代理程式。

## 已知問題和解決方法

**Blueprint階段未產生任務**

若要解除封鎖並繼續，請推動代理程式以產生工作。

**要推送到GitHub及從GitHub提取的按鈕無法運作**

請改為從開發階段下載專案ZIP檔案。

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

## 其他資源

* [Commerce Developer Agent概觀](https://developer.adobe.com/commerce/extensibility/developer-agent/)
* [Commerce Developer Agent快速入門](https://developer.adobe.com/commerce/extensibility/developer-agent/getting-started)
* [Commerce Developer Agent提示提示](https://developer.adobe.com/commerce/extensibility/developer-agent/prompting)
* [Commerce Developer Agent支援和回饋](https://developer.adobe.com/commerce/extensibility/developer-agent/support)
