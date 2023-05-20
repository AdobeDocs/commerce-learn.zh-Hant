---
title: 使用GraphQL
description: 瞭解有關在Adobe Commerce使用GraphQL執行突變的簡介 [!DNL Magento Open Source]。 使用POST調用執行第一個變異。
landing-page-description: 瞭解有關在Adobe Commerce使用GraphQL執行突變的簡介 [!DNL Magento Open Source]。 使用POST調用執行第一個變異。
short-description: 瞭解有關在Adobe Commerce使用GraphQL執行突變的簡介 [!DNL Magento Open Source]。 使用POST調用執行第一個變異。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 突變

任何完整的API規範不僅需要提供查詢資料的功能，還需要提供建立和更新資料的功能。

REST區分更改資料的請求和不使用請求類型或「verb」(GET與POST或PUT)的請求。
使用GraphQL時，資料修改查詢由 `mutation` 關鍵字，該關鍵字與在伺服器上定義的架構中的不同根類型相對應。

查看此示例變異，將產品添加到用戶的購物車。 (這要求為登錄客戶的會話生成的購物車ID或使用 `createEmptyCart` 突變。)

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

您可以想像，在請求中發送上述變異，同時發送以下變數字典：

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

最後，你可能會收到這樣的回復：

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

需要注意的是，關於上例，除了使用 `mutation` 關鍵字 `query`，語法與查詢相同。 與查詢一樣，變異包括：

* 任意操作名(`doAddToCart`)
* 變數清單(例如， `$cartId`)
* 初始欄位(`addProductsToCart`)(例如， `cartId`，設定為的值 `$cartId`括弧
* 大括弧中的欄位的子選擇

欄位子選擇允許您靈活定義要返回的欄位（從指定為返回值的類型） `addProductsToCart` - `AddProductsToCartOutput`)。

如前所述，在GraphQL架構中定義的欄位在查詢的根類型上開始(通常稱為 `Query`)。 同樣，也存在另一種根類型用於突變(通常稱為 `Mutation`)。 `addProductsToCart` 是該根類型上的欄位。

關於上例的其他幾點說明：

* 的 `!` 字元尾碼 `String` 和 `CartItemInput` 表示變數是必需的。
* 方括弧(`[]`) `CartItemInput` 指定的類型 `$cartItems` 指示該類型的清單，而不是單個值。

{{$include /help/_includes/graphql-rest-related-links.md}}
