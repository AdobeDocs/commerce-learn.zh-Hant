---
title: 分割付款POC：Commerce模組AI提示
description: 瞭解如何使用此提示來產生Client_SplitPayment。 REST、外掛程式、簽出JavaScript、I/O事件以及啟用、編譯和部署命令。
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# 分割付款POC：Commerce模組AI提示

使用此頁面來複製產生`Client_SplitPayment`處理中模組的完整提示： REST、工作階段處理、**[!UICONTROL Checkout]**&#x200B;以及針對分割付款概念證明顯示的&#x200B;**[!UICONTROL Admin]**。 運運算元工作流程停留在App Builder中。

## 如何使用這個提示

從&#x200B;**PROMPT START**&#x200B;到&#x200B;**提示**&#x200B;的結尾的所有內容都複製到游標（使用Claude）或直接複製到Claude。 從Commerce專案的根目錄或AI可建立檔案的目錄執行。

## 執行前自訂

* 將`Client`取代為您的真實廠商名稱。
* 如果您想要不同的模組名稱，請變更`SplitPayment`。
* 如果網站使用自訂主題，版面XML和RequireJS路徑可能需要變更。
* 如果您的&#x200B;**[!UICONTROL Cash on delivery]**&#x200B;方法使用與`cashondelivery`不同的程式碼，請更新`payment-method-helper.js`。


## 提示

**提示開始**

您正在產生完整的生產就緒Adobe Commerce 2.4.5+處理中模組，用於分割付款功能。 此模組是薄型PHP轉接器，可在Commerce生命週期的適當時間公開正確的REST曲面並附加正確的資料。 所有運運算元工作流程邏輯都存在於Adobe App Builder中（不在此模組中）。

**模組識別：**
* 廠商： `Client`
* 模組： `SplitPayment`
* 完整名稱： `Client_SplitPayment`
* 名稱空間： `Client\SplitPayment`
* 位置： `app/code/Client/SplitPayment/`
* 相依性： `Magento_Checkout`、`Magento_CustomerBalance`、`Magento_Sales`、`Magento_Quote`、`Magento_WebApi`、`Magento_AdobeCommerceEventsClient`

產生下列檔案結構中列出的每個檔案。 請勿忽略任何檔案。 在所有PHP檔案中使用`declare(strict_types=1)`。


### 要產生的檔案結構

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### 行為規格

#### &#x200B;1. 資料庫結構描述(`etc/db_schema.xml`)

將這些資料行新增至`sales_order` （資源： `sales`）：

| 欄 | 型別 | 可為空 | 註解 |
|---|---|---|---|
| `split_store_credit_amount` | decimal(12,4) | 是 | 存放區貸方部分 |
| `split_cash_amount` | decimal(12,4) | 是 | 到期現金部分 |
| `split_cash_status` | varchar(32) | 是 | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | 不帶正負號的int | 是 | 儲存貸方發票實體ID |
| `split_cash_invoice_id` | 不帶正負號的int | 是 | 現金商業發票實體ID |

也為這些資料行產生`db_schema_whitelist.json`。

#### &#x200B;2. 延伸屬性(`etc/extension_attributes.xml`)

將`split_store_credit_amount` （浮點數）、`split_cash_amount` （浮點數）、`split_cash_status` （字串）新增至：
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. REST端點(`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

所有三個對應到`Client\SplitPayment\Api\SplitPaymentManagementInterface`。

#### &#x200B;4. I/O事件(`etc/io_events.xml`)

訂閱`observer.sales_order_place_before`並包含欄位：
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — 工作階段包裝函式

將宣告的分割金額儲存在結帳工作階段中。 必須公開：
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — 傳回`['store_credit' => float, 'cash' => float]`
* `clear(): void`
* `beginBalanceApply(): void` — 設定在商店信用申請期間隱藏總修復外掛程式的旗標
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — REST控制器

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* 驗證購物車屬於目前的工作階段（比較數值和遮罩的報價ID）
* 在`SplitPaymentSession`中儲存金額
* 成功時傳回`true`；失敗時擲回具有一般訊息的`LocalizedException`

**`markCashReceived(int $orderId): bool`**
* 載入排序依據： `entity_id`
* 驗證`split_cash_status === 'pending'`
* 將狀態設為`received`，將狀態設為`processing`
* 新增歷程記錄註解： `"Cash payment of $X.XX received."`
* 呼叫`SplitInvoiceService::createCashPortionInvoice($orderId)`
* 以現金商業發票遞增識別碼新增註解
* 呼叫`createShipmentIfPossible($orderId)` — 如果有可出貨的料號，則使用`ShipOrder::execute()`建立出貨
* 傳回`true`；發生任何錯誤時擲回泛型`LocalizedException`

**`markCashDeclined(int $orderId): bool`**
* 載入順序
* 驗證`split_cash_status === 'pending'`
* 驗證`$order->canCancel()`
* 將狀態設定為`declined`，新增註解： `"Cash payment declined (simulated fraud check)."`
* 儲存訂單
* 呼叫`OrderManagement::cancel($orderId)`
* 傳回`true`；失敗時擲回泛型`LocalizedException`

#### 7. `PlaceOrderPlugin` — aroundPlaceOrder on `QuoteManagement`

這是最重要的外掛程式。 執行`aroundPlaceOrder`：

1. 載入報價；若未啟用，請立即呼叫`$proceed()`
2. 讀取`SplitPaymentSession::getAmounts()`
3. 如果報價單上的`customer_balance_amount_used > 0`，請呼叫`BalanceManagementInterface::remove($cartId)` （控制代碼`LocalizedException` — 記錄並作為一般訊息重傳）
4. 呼叫`recollectQuoteForBalanceOperation()` — 載入引號、呼叫`collectTotals()`、儲存（在`beginBalanceApply()` / `endBalanceApply()`內以隱藏總修復外掛程式）
5. 如果`$storeCredit > 0`，請呼叫`BalanceManagementInterface::apply($cartId, $storeCredit)` （控制代碼失敗）
6. 重新載入報價；設定擴充功能屬性： `split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`
7. 將`payment->additional_information['client_split_payment']`儲存為`['store_credit' => x, 'cash' => y]`
8. 儲存報價
9. 呼叫`$proceed()`，擷取訂單識別碼
10. 撥打`SplitPaymentSession::clear()`
11. 退貨單ID

#### &#x200B;8. `CopySplitPaymentToOrder` — `sales_model_service_quote_submit_before`的觀察者

從作業階段讀取分割金額→付款additional_information→報價擴充屬性（按該優先順序）。 將`split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`寫入順序物件。

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — `sales_order_place_after`的觀察者

下單後，如果訂單有商店信用金額(`split_store_credit_amount > 0`)，請致電`SplitInvoiceService::createStoreCreditPortionInvoice($orderId)`，立即為商店信用部份開立發票。

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
僅建立商店貸方部分的商業發票。 將商業發票上的`customer_balance_amount`設為商店貸方金額。 註冊並儲存發票。 失敗時傳回發票實體ID或Null （記錄；不要重新擲回）。

**`createCashPortionInvoice(int $orderId): ?int`**
建立現金部分的商業發票（SC商業發票未涵蓋的剩餘訂單專案/金額）。 註冊並儲存。 失敗時傳回實體ID或Null。

#### &#x200B;11. `CheckoutPlugin` — 在`PaymentInformationManagement`

`Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`上的外掛程式。 繼續之前：
* 從結帳工作階段載入報價單
* 取得設定的閾值： `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`，預設`100`
* 若`$quote->getGrandTotal() > $threshold`，擲回`LocalizedException('Payment could not be processed. Please try again or contact support.')`

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — 共`Customerbalance`個

在原生客戶餘額總計收集執行之後，上限`customer_balance_amount_used`到`SplitPaymentSession::getAmounts()['store_credit']`。 這可防止Commerce在客戶宣告較小的商店信用部分時，過度套用完整的客戶餘額。

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — 在`Quote\Address\Total\Grand`

收集總計之後：如果分割付款工作階段存在且`isBalanceApplyInProgress()`為false，請將報價總計設定為工作階段現金金額。 如此一來，結帳UI只會顯示現金的到期專案。

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — 於`Sales\Model\Order\Invoice`

在收集商業發票總計後，如果商業發票的相關訂單有`split_sc_invoice_id`，請更正商業發票上的`customer_balance_amount`，以防止重複沖銷商店貸方。

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — 在`Payment\Model\Checks\ZeroTotal`

在`SplitPaymentSession::getAmounts()['cash'] > 0`時允許COD付款，即使報價總計為$0 （因為商店信用涵蓋整個訂單）。

#### &#x200B;16. `LayoutProcessorPlugin` — 在`Checkout\Block\Checkout\LayoutProcessor`

處理配置後：
* 將`Client_SplitPayment/js/view/payment/split-payment`元件插入`components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`處`cashondelivery`付款方式元件的`additional`子系
* 從付款步驟移除原生商店點數UI元件(`customerBalance`， `useStoreCredit`) — 分割付款元件擁有商店點數顯示/應用程式

#### &#x200B;17. `OrderRepositoryPlugin` — 於`OrderRepositoryInterface`

在`get()`和`getList()`之後，從一般`sales_order`欄(`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`)中水合化訂單的延伸屬性。

#### &#x200B;18. `AdminSplitPaymentTitlePlugin` — 在`Payment\Block\Info`

在`getTitle()`傳回之後，如果付款方式為`cashondelivery`且訂單具有分割付款，請將`" (Split: Cash $X.XX + Store Credit $Y.YY)"`附加至標題。

#### &#x200B;19. `OrderPaymentPlugin` — 在`Sales\Block\Adminhtml\Order\Payment`

在`_beforeToHtml`中或透過afterToHtml，在Commerce管理員訂單檢視中將分割付款詳細資料（現金金額、商店信用金額、狀態）附加至付款區塊HTML。

#### &#x200B;20. 店面商店貸方餘額控制器

`Controller/Checkout/StoreCreditBalance.php` — 路由： `GET /splitpayment/checkout/storecreditbalance`

傳回JSON： `{"balance": float, "logged_in": bool}`。 如果客戶未登入，則傳回`{"balance": 0, "logged_in": false}`。 從`Magento\CustomerBalance\Model\Balance`讀取餘額。

透過`frontName="splitpayment"`在`etc/frontend/routes.xml`中註冊前端路由。

#### &#x200B;21. 簽出ClokoutJS元件 — `split-payment.js`

一個`uiComponent`，該項：
* 偵測何時選取`cashondelivery`付款方式(`quote.paymentMethod.subscribe`)
* 透過`GET /splitpayment/checkout/storecreditbalance`載入客戶的商店信用餘額
* 以完整訂單總計（從`total_segments`計算，不包括`grand_total`和`customerbalance`）預先填入現金金額欄位 — 絕對不會直接使用`grand_total`，因為它可設為現金餘額)
* 當客戶變更現金金額輸入時：計算所需的存放區銷退折讓=訂單總計−現金；驗證；過帳至`POST /V1/split-payment/set`
* 顯示下列專案的驗證訊息：現金>訂單總計、商店信用不足，未登入
* 輸入有效分割時顯示成功訊息： `"The remaining $X.XX will automatically be applied from your store credit."`
* 選取其他付款方式時重設（張貼`{storeCreditAmount: 0, cashAmount: 0}`以清除工作階段）

#### 22. `cashondelivery-method.js`

延伸`Magento_OfflinePayments/js/view/payment/offline-payments`。 使用`payment-method-helper.js`偵測現金方式代碼。 在其`additional`區域中登入`split-payment`元件。

#### 23. `payment-method-helper.js`

公用程式傳回`getCashMethodCode()` — 檢查`cashondelivery`的`window.checkoutConfig.paymentMethods`；如有需要，將退回至`checkmo`。

#### &#x200B;24. `cashondelivery.html` 範本

標準COD範本，但包含`<!-- ko foreach: getRegion('additional') -->`區域，以便呈現分割付款子元件。

#### &#x200B;25. `split-payment.html` 範本

分割付款欄位的去底色JS範本：
* 可用的商店貸方餘額顯示
* 現金金額輸入（數字，步驟0.01）
* 儲存點數部分顯示（唯讀）
* 自動套用商店點數訊息（分割有效且商店點數> 0時顯示）
* 驗證錯誤訊息

#### 26. `requirejs-config.js`

地圖：
* 元件→的`Client_SplitPayment/js/view/payment/split-payment`
* `Client_SplitPayment/js/view/payment/cashondelivery-method`→COD覆寫
* `Client_SplitPayment/js/model/payment-method-helper`→協助程式

#### 27. `etc/config.xml`

預設系統設定值：

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### 重要實作注意事項

**存放區信用申請必須使用`BalanceManagementInterface`，而非直接模型操作。** `BalanceManagementInterface::apply()`會自動處理工作階段、驗證及購物車重新計算。

**`PlaceOrderPlugin`必須使用`aroundPlaceOrder` （不是`beforePlaceOrder`）。** 必須在購物車仍在使用中時套用商店點數，而且必須在呼叫`$proceed()`之前保證。

**`beginBalanceApply` / `endBalanceApply`的工作階段旗標模式是關鍵的。** 若沒有它，`FixSplitPaymentGrandTotalPlugin`會在餘額作業內的`collectTotals()`期間執行，並將總計設定為現金餘額，導致`BalanceManagementInterface::apply()`失敗或限制貸方。

**絕不向客戶公開內部錯誤詳細資料。** 所有REST回應的`catch`區塊都必須擲回`LocalizedException('Payment could not be processed. Please try again or contact support.')`。

**`entity_id`是數值資料庫識別碼。** 來自App Builder的REST呼叫一律使用`entity_id`，而非`increment_id`。

**`SplitInvoiceService`應該攔截並記錄錯誤，而不是傳播它們。** 發票建立失敗不應取消已下訂的訂單 — 請記錄失敗並讓管理員手動處理。


### 產生檔案之後

在Commerce專案根目錄中執行這些命令：

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 提示結尾


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
