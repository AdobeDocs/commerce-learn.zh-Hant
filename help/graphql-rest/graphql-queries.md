---
title: 使用GraphQL執行查詢
description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 以下為使用GET和POST呼叫的GraphQL簡介。
landing-page-description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 以下為使用GET和POST呼叫的GraphQL簡介。
short-description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 以下為使用GET和POST呼叫的GraphQL簡介。
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# GraphQL查詢

這是GraphQL和Adobe Commerce系列的第2部分。 在本教學課程和影片中，瞭解GraphQL查詢，以及如何針對Adobe Commerce執行這些查詢。

>[!VIDEO](https://video.tv.adobe.com/v/3424120?learn=on)

## 本系列中GraphQL的相關影片和教學課程

* [第1部分GraphQL — 簡介](../graphql-rest/intro-graphql.md)
* [第3部分GraphQL — 變動](../graphql-rest/graphql-mutations.md)
* [第4部分GraphQL — 結構描述](../graphql-rest/graphql-schema.md)

## GraphQL語法範例

以完整的範例直接探討GraphQL查詢語法。 (請記住，您可以對https://venia.magento.com/graphql自行嘗試此做法。)

請注意下列GraphQL查詢，此查詢會依件細分：

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

GraphQL伺服器對上述查詢的合理回應可能是：

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

上述範例仰賴伺服器所定義的適用於Adobe Commerce的現成GraphQL架構。 在此請求中，您一次可查詢多種型別的資料。 查詢會精確表示您想要的欄位，而傳回資料的格式與查詢本身類似。

>[!NOTE]
>
>GraphQL使用者端會模糊化實際傳送之HTTP請求的形式，但很容易發現。 如果您使用瀏覽器型使用者端，請遵守 [!UICONTROL Network] 索引標籤傳送查詢。 您會看到請求包含原始內文，其中包含「query： `{string}`「」，其中 `{string}` 只是整個查詢的原始字串。 如果請求是以GET形式傳送，則查詢可能會改以查詢字串引數&quot;query&quot;編碼。 不像REST，HTTP要求型別並不重要，只有關鍵查詢的內容。


## 查詢您想要的

`country` 和 `categories` 在此範例中，針對兩種不同的資料，代表兩個不同的「查詢」。 不同於類似REST的傳統API範例，後者會針對每種資料型別定義個別且明確的端點。 GraphQL可讓您透過運算式查詢單一端點，以便一次擷取多種型別的資料。

同樣，查詢會明確指定兩者所需的欄位 `country` (`id` 和 `full_name_english`)和 `categories` (`items`，而您收到的回傳資料會反映該欄位規格。 這些資料型別可能還有許多欄位可供使用，但您只能取回您要求的內容。


>[!NOTE]
>
>您可能會注意到 `items` 實際上是 _陣列_ ，但您還是直接選取其子欄位。 當欄位的型別是清單時，GraphQL會隱含地瞭解要套用至清單中每個專案的子選取範圍。

## 引數

雖然您要傳回的欄位是在每個型別的大括弧內指定，但已命名的引數和它們的值是在型別名稱后的括弧內指定。 引數通常是選用的，通常會影響查詢結果的篩選、格式化或以其他方式轉換的方式。

您正在傳遞 `id` 引數至 `country`，指定要查詢的特定國家/地區，以及 `filters` 的引數 `categories`.

## 欄位一直向下對齊

雖然您可能會考慮 `country` 和 `categories` 作為單獨的查詢或實體，查詢中表示的整個樹狀結構實際上只包含欄位。 的運算式 `products` 語法上與以下專案並無不同 `categories`. 兩者都是欄位，其結構之間沒有差異。

任何GraphQL資料圖表都有單一的「根」型別(通常稱為 `Query`)以啟動樹狀結構，且通常視為實體的型別會指派給此根目錄上的欄位。 範例查詢實際上是為根型別建立一個一般查詢，並選取欄位 `country` 和 `categories`. 接著，它會選取這些欄位的子欄位，依此類推，可能深入數個層級。 只要欄位的傳回型別是複雜型別（例如，具有自己欄位的型別，而不是純量型別），請繼續選取您想要的欄位。

這個巢狀欄位的概念也是您傳送引數的原因所在 `products` (`pageSize` 和 `currentPage`)，其方式與您針對最上層所做的相同 `categories` 欄位。

![GraphQL欄位樹](../assets/graphql-field-tree.png)

## 變數

請嘗試不同的查詢：

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

首先要注意的是，已新增關鍵字 `query` 在查詢的開頭大括弧之前，加上作業名稱(`getProducts`)。 此作業名稱是任意的；它未對應到伺服器結構描述中的任何內容。 已新增此語法以支援變數的引入。

在上一個查詢中，您可以直接將欄位引數的值硬式編碼，格式為字串或整數。 然而，GraphQL規格具有第一級支援，可透過變數將使用者輸入與主要查詢分開。

在新查詢中，您在整個查詢的開頭大括弧前使用括弧來定義 `$search` 變數（變數一律使用美元符號前置詞語法）。 此變數會提供給 `search` 的引數 `products`.

當查詢包含變數時，GraphQL要求應隨查詢本身包含個別的JSON編碼值字典。 對於上述查詢，除了查詢本文之外，您還可以傳送下列變數值JSON：

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您是針對Venia範例網站(而不是您自己的Adobe Commerce執行個體)嘗試這些查詢，則傳回的結果可能會是空的 `related_products`.

在您用於測試的任何GraphQL感知使用者端（例如Altair和GraphiQL）中，UI支援與查詢分開輸入變數JSON。

就像您看到GraphQL查詢的實際HTTP請求包含「query： `{string}`在其內文中，任何包含變數字典的要求僅包含額外的「變數： `{json}`」在同一主體中，其中 `{json}` 是包含變數值的JSON字串。

新查詢也使用 _片段_ (`productDetails`)重複使用在多個位置的相同欄位選取範圍。 [閱讀更多有關片段的資訊](https://graphql.org/learn/queries/#fragments){target="_blank"} (位於GraphQL檔案中)。

{{$include /help/_includes/graphql-rest-related-links.md}}
