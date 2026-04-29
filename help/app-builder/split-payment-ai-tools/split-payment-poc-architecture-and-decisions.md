---
title: 分割付款POC：架構與設計決策
description: 瞭解分割付款POC如何將同步結帳對應至Commerce，以及如何將I/O導向步驟對應至App Builder，並搭配擴充功能屬性、REST和五個外掛程式邊緣案例。
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# 分割付款POC：架構與設計決策

本頁說明分割付款概念證明背後的架構選擇。 請先閱讀本檔案再在本系列中使用建置提示，以瞭解每個元件的結構方式，以及如何在您自己的專案中調整模式。

## 核心原則

概念證明並不是關於最優雅的分割付款實施。 此影片說明&#x200B;**如何開始將Commerce邏輯移至App Builder，而不需要突然重寫**。

在整個中套用的規則是：

> **如果某些專案必須在Commerce要求週期中同步執行，或必須叫用沒有乾淨外部表面的Commerce內部API，則會保留在PHP中。 其他所有專案都會移至App Builder。**

## Commerce (PHP)中的內容及原因

### &#x200B;1. 儲存信用申請： `PlaceOrderPlugin`

使用`Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`將商店點數套用至購物車。 此方法僅適用於&#x200B;**使用中**&#x200B;購物車。 當下訂單時，購物車即會停用。 App Builder會在下訂單&#x200B;*後收到I/O事件*，因此無法從App Builder套用商店點數。

**課程：**&#x200B;在Commerce中必須執行任何必須變更購物車狀態，才能下訂單。 沒有因應措施。

### &#x200B;2. 同步臨界值保護： `CheckoutPlugin`

$100訂單臨界值檢查必須在客戶選擇&#x200B;**[!UICONTROL Place Order]**&#x200B;之前，於付款步驟封鎖客戶。 在Commerce要求週期中，回應必須同步。 App Builder是事件導向和非同步的，因此當下無法傳回立即錯誤。

App Builder *也*&#x200B;會驗證臨界值（稽核），但客戶體驗取決於先執行的Commerce檢查。

### &#x200B;3. 自訂REST端點： `webapi.xml`和`SplitPaymentManagement`

下列端點必須：

* 叫用`SplitInvoiceService` （使用Commerce內部發票服務的發票）
* 叫用`ShipOrder::execute()` （Commerce的內部出貨服務）
* 使用Commerce的訂單狀態機器更新訂單狀態和狀態

`/V1/split-payment/orders/:id/cash-received`和`/V1/split-payment/orders/:id/cash-decline`

該行為沒有乾淨的公用REST層，因此Commerce會公開端點。 App Builder會呼叫它們。

### &#x200B;4. 分割報價與訂單金額：觀察者與外掛程式

Commerce需要訂單上的分割量(`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`)，這對App Builder讀取的REST回應和&#x200B;**[!UICONTROL Admin]**&#x200B;訂單檢視都是如此。 金額附加了擴充屬性，並從`sales_model_service_quote_submit_before`上的報價複製到觀察者的訂單中。

## App Builder中有哪些元素及其原因

### &#x200B;1. 事件導向的訂單處理： `payment-orchestrator`

在`sales_order_place_before`引發後，App Builder會接收事件。 它會重新驗證臨界值（作為稽核）、記錄訂單上的&#x200B;*待付現金*&#x200B;註解，以及更新訂單狀態。 這些都不需要新的PHP，只有REST返回Commerce。

### &#x200B;2. 現金允收： `payment-accept`

當ERP （或儀表板中的操作員）確認收到現金時，`payment-accept`會呼叫`POST /V1/split-payment/orders/:id/cash-received`。 在Commerce中處理發票、出貨及訂單狀態。 App Builder為觸發因子。

### &#x200B;3. 現金拒絕： `payment-decline`

`payment-decline`呼叫`POST /V1/split-payment/orders/:id/cash-decline`且Commerce取消訂單。 與現金承兌的模式相同。

### &#x200B;4. 操作員儀表板： `demo-dashboard`

由App Builder Web動作提供的獨立HTML控制面板。 它會從Commerce REST擷取等候現金的訂單，並提供呼叫上述App Builder動作的&#x200B;**[!UICONTROL Accept]** / **[!UICONTROL Decline]**&#x200B;動作。 Commerce **[!UICONTROL Admin]**&#x200B;不是必要專案。

## 臨界值：故意強制執行兩次

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce擁有面對使用者的防護；App Builder擁有置入後稽核。** 這都是有意為之。

## 商店點數：為什麼它停留在PHP中

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

Orchestrator中的`store-credit.js`檔案會記錄此內容。 這是沒有操作的Stub，其中包含可解釋為何沒有使用的註解。

## 延伸屬性：粘結

分割金額在擴充屬性上透過系統移動：

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## 資料模型

此模組新增的&#x200B;**`sales_order`個平面資料行**

| 欄 | 型別 | 用途 |
| --- | --- | --- |
| `split_store_credit_amount` | 浮點數 | 已套用的商店點數 |
| `split_cash_amount` | 浮點數 | 應付現金金額 |
| `split_cash_status` | varchar | `pending`、`received`或`declined` |
| `split_sc_invoice_id` | int | 商店貸方發票的實體ID |
| `split_cash_invoice_id` | int | 現金商業發票的實體識別碼 |

**延伸屬性** （在`CartInterface`、`OrderInterface`和`OrderPaymentInterface`）

* `split_store_credit_amount` (float)
* `split_cash_amount` (float)
* `split_cash_status` (string)

## I/O event payload fields

`observer.sales_order_place_before` is configured in `io_events.xml` to include the following in the event:

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder uses `entity_id` as the order ID and `split_store_credit_amount` and `split_cash_amount` for threshold validation.

## The five edge cases the proof of concept covers

### 1. `CapCustomerBalanceCollectPlugin`

Commerce’s native **[!UICONTROL Customer balance]** total collector can over-apply (it can see the full available balance, not the session-declared split amount). This plugin caps the amount to the value declared in the session.

### 2. `FixSplitPaymentGrandTotalPlugin`

After store credit is applied, the quote **[!UICONTROL Grand Total]** can drop to the cash-only amount. The checkout JavaScript must compute the order total for split validation *before* that change. The plugin runs after totals collection and corrects the display, while the JavaScript does not trust `grand_total` alone and reconstructs the value from subtotal segments.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

When invoice totals are recollected, store credit can be applied twice. This plugin corrects `customer_balance_amount` on invoices.

### 4. `SplitPaymentZeroTotalPlugin`

After store credit is applied, the cart **[!UICONTROL Grand Total]** can be $0 (full store credit order). Commerce’s **[!UICONTROL Zero subtotal checkout]** check can block COD in that case. This plugin allows COD when the session cash amount is greater than 0.

### 5. Quote recollection before `BalanceManagementInterface::apply()`

`apply()` checks the amount against the current **[!UICONTROL Grand Total]**. If the total is already the cash portion only, `apply()` can fail or cap. `PlaceOrderPlugin` temporarily suspends the grand-total fix while balance is applied, using a session flag (`beginBalanceApply` / `endBalanceApply`).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
