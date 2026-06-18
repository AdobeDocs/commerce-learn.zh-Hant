---
title: 分割付款POC — 架構與設計決策
description: 瞭解分割付款POC如何將同步結帳對應至Commerce，以及如何將I/O導向步驟對應至App Builder，並搭配擴充功能屬性、REST和五個外掛程式邊緣案例。
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
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

* `split_store_credit_amount` （浮點數）
* `split_cash_amount` （浮點數）
* `split_cash_status` （字串）

## I/O事件裝載欄位

在`io_events.xml`中將`observer.sales_order_place_before`設定為包含事件中的下列專案：

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder使用`entity_id`作為訂單ID，並使用`split_store_credit_amount`和`split_cash_amount`進行臨界值驗證。

## 概念證明涵蓋的五個邊緣案例

### 1. `CapCustomerBalanceCollectPlugin`

Commerce的原生&#x200B;**[!UICONTROL Customer balance]**&#x200B;總計收集器可以超額套用（它可以看到完整的可用餘額，而不是工作階段宣告的分割量）。 此外掛程式會將數量限製為作業階段中宣告的值。

### 2. `FixSplitPaymentGrandTotalPlugin`

在套用商店點數後，報價單&#x200B;**[!UICONTROL Grand Total]**&#x200B;可以放入僅限現金的金額。 簽出JavaScript必須計算變更前&#x200B;*分割驗證*&#x200B;的訂單總計。 外掛程式會在總計集合之後執行，並修正顯示，而JavaScript不只信任`grand_total`，而且會從小計區段重新建構值。

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

在收回商業發票總計時，可以套用兩次商店點數。 此外掛程式更正發票上的`customer_balance_amount`。

### 4. `SplitPaymentZeroTotalPlugin`

套用商店點數後，購物車&#x200B;**[!UICONTROL Grand Total]**&#x200B;可以是$0 （完整商店點數訂單）。 在這種情況下，Commerce的&#x200B;**[!UICONTROL Zero subtotal checkout]**&#x200B;檢查可以封鎖COD。 當工作階段現金金額大於0時，此外掛程式可允許進行COD。

### &#x200B;5. `BalanceManagementInterface::apply()`之前的引號回收

`apply()`對照目前的&#x200B;**[!UICONTROL Grand Total]**&#x200B;檢查金額。 如果總計已經是僅現金部分，`apply()`可能會失敗或達到上限。 `PlaceOrderPlugin`使用工作階段旗標(`beginBalanceApply` / `endBalanceApply`)，在套用餘額時暫時暫停總計修正。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
