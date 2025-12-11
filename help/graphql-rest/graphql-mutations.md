---
title: 使用GraphQL執行變異
description: 取得在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL執行突變的簡介。 使用POST呼叫執行您的第一個變異。
landing-page-description: 取得在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL執行突變的簡介。 使用POST呼叫執行您的第一個變異。
short-description: 取得在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL執行突變的簡介。 使用POST呼叫執行您的第一個變異。
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# 突變

這是GraphQL和Adobe Commerce系列的第3部分。 變動是指使用GraphQL儲存、更新和傳回值的能力。


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## 本系列中GraphQL的相關影片和教學課程

* [第1部分GraphQL — 簡介](../graphql-rest/intro-graphql.md)
* [第2部分GraphQL — 查詢](../graphql-rest/graphql-queries.md)
* [第4部分GraphQL — 結構描述](../graphql-rest/graphql-schema.md)

## 變異範例

任何完整的API規格不僅需要提供查詢資料的能力，還需要提供建立和更新資料的能力。

REST會區分變更資料的請求與不符合請求型別或「動詞」(GET與POST或PUT)的請求。
使用GraphQL時，資料修改查詢會以對應至其他關鍵字的`mutation`關鍵字加以區分
在伺服器上定義的結構描述中的根型別。

檢視這個將產品新增到使用者購物車的變異範例。 (這需要產生的購物車ID
用於登入客戶的工作階段或使用`createEmptyCart`突變。)

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

您可以想像上述變異會連同下列變數字典一起在要求中傳送：

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

關於上述範例，需要注意的主要事項是，除了使用`mutation`關鍵字而非`query`之外，
語法與查詢相同。 就像查詢一樣，變異包括：

* 任意操作名稱(`doAddToCart`)
* 變數清單（例如，`$cartId`）
* 括弧中有引數的初始欄位(`addProductsToCart`) （例如，`cartId`，設定為`$cartId`的值）
* 大括弧中的欄位細選

欄位子選項可讓您靈活地定義想要傳回的欄位(從指派為
完成變異後傳回值`addProductsToCart` - `AddProductsToCartOutput`)。

如前所述，GraphQL結構描述中定義的欄位會從查詢的根型別開始（通常稱為`Query`）。 同樣地，
存在另一個根型別用於變動（通常稱為`Mutation`）。 `addProductsToCart`為欄位
在該根型別上。

關於上述範例的其他注意事項：

* `!`字元尾碼`String`和`CartItemInput`表示變數為必要專案。
* 為`[]`指定的`CartItemInput`型別周圍的方括弧(`$cartItems`)表示清單
而不是單一值。

{{$include /help/_includes/graphql-rest-related-links.md}}
