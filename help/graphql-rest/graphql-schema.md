---
title: 架構語言與GraphQL
description: 瞭解與GraphQL有關的架構。 閱讀架構的說明，以及一些有趣的模式和讀取架構的方法。
landing-page-description: 這是GraphQL的簡介。 瞭解模式以及如何解釋某些元素
short-description: 這是GraphQL的簡介。 瞭解模式以及如何解釋某些元素
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# 架構語言

所使用的查詢和突變依賴於伺服器上正在實現的特定資料圖形，GraphQL運行時使用該資料圖形並用於解決查詢。 GraphQL規範定義了一種不可知語言，用於表示資料圖的類型和關係。

下面是一個縮寫類型架構，它支援您到目前為止所查看的查詢和突變：

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

你可以深入 [GraphQL檔案](https://graphql.org/learn/schema/){target="_blank"} 瞭解類型系統的詳細資訊，包括此處未表示的某些概念的語法。 然而，上例是不言自明的。 （另請注意語法與查詢語法的相似程度。） 定義GraphQL模式只是表示給定類型的可用參數和欄位以及這些欄位的類型。 每個複雜欄位類型本身必須有定義，等等，通過樹，直到您得到簡單的標量類型，如 `String`。

的 `input` 聲明在所有方面都像 `type` 但定義可用作參數輸入的類型。 另請注意 `interface` 聲明。 此功能與PHP中的介面或多或少相同。 其它類型從此介面繼承。

語法 `[CartItemInput!]!` 看起來很棘手，但最終還是相當直觀的。 的 `!` _內_ 括弧聲明陣列中的每個值必須為非null，而 _外_ 聲明陣列值本身必須為非null（例如，空陣列）。

>[!NOTE]
>
>用於根據模式獲取和格式化資料的邏輯以及這種邏輯如何映射到特定類型的邏輯取決於GraphQL運行時實現。 但是，實施應遵循一種概念流，這一概念流對於對嵌套欄位的理解是有意義的：與根關聯的解析操作 `Query` 或 `Mutation` 執行類型，檢查請求中指定的每個欄位。 對於解析為複雜類型的每個欄位，都會對該類型執行類似的解析，等等，直到所有內容都解析為標量值。

{{$include /help/_includes/graphql-rest-related-links.md}}
