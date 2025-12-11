---
title: GraphQL的結構描述語言
description: 瞭解與GraphQL有關的結構描述。 閱讀結構描述，以及一些有趣的模式和讀取結構描述的方法。
landing-page-description: 以下是GraphQL的簡介。 瞭解結構以及如何解譯部分元素
short-description: 以下是GraphQL的簡介。 瞭解結構以及如何解譯部分元素
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---

# 結構描述語言

這是GraphQL和Adobe Commerce系列的第4部分。 使用的查詢和變動取決於在伺服器上實作的特定資料圖表，GraphQL執行階段會使用這個圖表並用來解決查詢。 GraphQL規格定義了不可知的語言，用於表示資料圖表的型別和關係。

>[!VIDEO](https://video.tv.adobe.com/v/3446621?captions=chi_hant&learn=on)

## 本系列中GraphQL的相關影片和教學課程

* [第1部分GraphQL — 簡介](../graphql-rest/intro-graphql.md)
* [第2部分GraphQL — 查詢](../graphql-rest/graphql-queries.md)
* [第3部分GraphQL — 變動](../graphql-rest/graphql-mutations.md)

## 結構描述範例

以下是支援您目前已檢視之查詢和變動的縮寫型別結構描述：

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

您可以深入探討[GraphQL檔案](https://graphql.org/learn/schema/){target="_blank"}，瞭解型別系統的詳細資訊，包括此處未呈現之部分概念的語法。 然而，上述範例不言自明。 （此外，請注意語法與查詢語法的相似程度。） 定義GraphQL結構描述只是表示給定型別的可用引數和欄位以及這些欄位的型別的問題。 每個複雜欄位型別本身都必須有定義，依此類推，透過樹狀結構，直到您取得簡單純量型別（例如`String`）為止。

`input`宣告在所有方面與`type`類似，但定義了可以做為引數輸入的型別。 同時記下`interface`宣告。 此功能與PHP中的介面大致相同。 其他型別會繼承自此介面。

語法`[CartItemInput!]!`看起來很棘手，但最後是相當直覺的。 `!` _在_&#x200B;內，括弧宣告陣列中的每個值都必須是非Null，而&#x200B;_在_&#x200B;外宣告陣列值本身必須是非Null （例如，空陣列）。

>[!NOTE]
>
>如何根據結構描述擷取及格式化資料，以及如何將這類邏輯對應至特定型別的邏輯，取決於GraphQL執行階段實施。 不過，實作應遵循概念流程，根據對巢狀欄位的瞭解，這個流程才有意義：執行與根`Query`或`Mutation`型別相關聯的解析作業，檢查要求中指定的每個欄位。 對於每個解析為複雜型別的欄位，會對該型別進行類似的解析，以此類推，直到所有內容都解析為純量值為止。

{{$include /help/_includes/graphql-rest-related-links.md}}
