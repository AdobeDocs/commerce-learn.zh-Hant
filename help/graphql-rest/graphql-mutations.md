---
title: 使用GraphQL執行變異
description: 了解如何在Adobe Commerce上使用GraphQL執行變異，以及 [!DNL Magento Open Source]. 使用POST呼叫執行第一個變異。
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL執行變異，以及 [!DNL Magento Open Source]. 使用POST呼叫執行第一個變異。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 0fa7ba038f542172c47bea859f8712759fcc52f7
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# 突變

任何完整的API規格都不僅要提供查詢資料的功能，還要能建立和更新資料。

REST會區分變更資料的請求與不變更請求類型或「verb」(GET與POST或PUT)的請求。
使用GraphQL時，資料修改查詢可由 `mutation` 與伺服器上定義的架構中不同根類型對應的關鍵字。

查看此示例變異，以將產品添加到用戶的購物車中。 (這需要針對登入客戶工作階段或使用 `createEmptyCart` 突變)。

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

您可以想像上述變異會連同下列變數字典一起傳送至要求中：

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

最後，您可能會收到如下的回應：

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

請注意，關於上述範例，除了使用 `mutation` 關鍵字而非 `query`，語法與查詢相同。 與查詢一樣，變異包括：

* 任意操作名(`doAddToCart`)
* 變數清單(例如 `$cartId`)
* 初始欄位(`addProductsToCart`)(例如， `cartId`，則設為的值 `$cartId`括弧中)
* 大括弧中欄位的子選項

欄位子選取功能可讓您彈性定義要傳回的欄位（從指定為傳回值的類型） `addProductsToCart` - `AddProductsToCartOutput`)，在突變完成後。

如前所述，GraphQL架構中定義的欄位會從查詢的根類型開始(通常稱為 `Query`)。 同樣，變異也存在另一種根類型(通常稱為 `Mutation`)。 `addProductsToCart` 是該根類型上的欄位。

上述範例的其他一些附註：

* 此 `!` 字元尾碼 `String` 和 `CartItemInput` 指出變數為必要項目。
* 方括弧(`[]`) `CartItemInput` 為指定的類型 `$cartItems` 指出該類型的清單，而非單一值。

{{$include /help/_includes/graphql-rest-related-links.md}}
