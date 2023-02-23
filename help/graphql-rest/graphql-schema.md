---
title: 使用GraphQL的結構語言
description: 了解與GraphQL相關的架構。 閱讀架構的說明，以及一些有趣的模式和讀取架構的方式。
landing-page-description: 這是GraphQL的簡介。 了解結構以及如何解譯部分元素
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 0fa7ba038f542172c47bea859f8712759fcc52f7
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# 綱要語言

使用的查詢和變異取決於伺服器上正在實作的特定資料圖表，GraphQL執行階段會利用該圖表並用來解析查詢。 GraphQL規格定義了一種不可知語言，用於表達資料圖表的類型和關係。

以下是簡略的類型結構，可支援您目前所研究的查詢和變異：

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

你可以深入研究 [GraphQL檔案](https://graphql.org/learn/schema/){target="_blank"} 了解類型系統的詳細資訊，包括此處未表示之部分概念的語法。 然而，上述例子是不言自明的。 （另請注意，查詢語法的語法有多相似。） 定義GraphQL結構只是表達指定類型的可用參數和欄位以及這些欄位類型的問題。 每個複雜的欄位類型本身必須有定義，依此類推，通過樹，直到得到像這樣的簡單標量類型 `String`.

此 `input` 聲明在所有方面都像 `type` 但會定義可用來作為引數輸入的類型。 另請注意 `interface` 聲明。 此功能與PHP中的介面幾乎相同。 其他類型則從此介面繼承。

語法 `[CartItemInput!]!` 看起來很棘手，但最終卻很直觀。 此 `!` _in_ 括弧聲明陣列中的每個值都必須為非空值，而括弧中的 _外部_ 聲明陣列值本身必須為非空值（例如，空陣列）。

>[!NOTE]
>
>根據結構擷取資料和格式化的邏輯，以及此邏輯如何對應至特定類型，由GraphQL執行階段實作決定。 但是，實作應遵循概念流程，這項流程有理有據，因為您對巢狀欄位有所了解：與根關聯的解析操作 `Query` 或 `Mutation` 會執行類型，檢查請求中指定的每個欄位。 對於解析為複雜類型的每個欄位，會對該類型等執行類似的解析，直到所有內容都解析為標量值。

{{$include /help/_includes/graphql-rest-related-links.md}}
