---
title: 使用GraphQL執行查詢
description: 了解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 此為使用GET和POST呼叫的GraphQL的簡介。
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 此為使用GET和POST呼叫的GraphQL的簡介。
short-description: Learn how to perform a query using GraphQL on Adobe Commerce and [!DNL Magento Open Source]. This is an introduction to GraphQL using GET and POST calls.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 0%

---

# GraphQL查詢

讓我們以完整的範例，直接探討GraphQL查詢語法。 (請記住，您可以針對https://venia.magento.com/graphql自行嘗試。)

請觀察下列GraphQL查詢，此查詢會逐件劃分：

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

來自GraphQL伺服器上上述查詢的可信回應可能是：

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

上述範例需仰賴伺服器上定義的Adobe Commerce現成可用的GraphQL架構。 在此請求中，您一次查詢多種資料類型。 查詢完全表示您想要的欄位，而傳回的資料的格式與查詢本身類似。

>[!NOTE]
>
>GraphQL用戶端會模糊化實際傳送之HTTP要求的形式，但很容易發現。 如果您使用瀏覽器型用戶端，請觀察 [!UICONTROL Network] 標籤。 您會看到要求包含由「查詢： `{string}`&quot;，其中 `{string}` 只是整個查詢的原始字串。 如果要求以GET形式傳送，則可能會改以查詢字串參數「query」編碼查詢。 與REST不同，HTTP要求類型不重要，只有查詢的內容。


## 查詢所需內容

`country` 和 `categories` 在此範例中，代表兩種不同的「查詢」，適用於兩種不同的資料。 與REST這類傳統API範例不同，REST可為每個資料類型定義個別和明確的端點。 GraphQL可讓您彈性地使用可一次擷取許多類型資料的運算式來查詢單一端點。

同樣，查詢也準確指定了兩者所需的欄位 `country` (`id` 和 `full_name_english`)和 `categories` (`items`，它本身有欄位的子選項)，而您接收到的資料則鏡像該欄位規範。 這些資料類型大概還有許多欄位可用，但您只會收到所要求的內容。


>[!NOTE]
>
>您可能會注意到 `items` 實際上 _陣列_ 的值，但您仍會直接為其選取子欄位。 當欄位的類型為清單時，GraphQL隱含地了解要套用至清單中每個項目的子選取項目。

## 引數

要返回的欄位在每種類型的大括弧內指定，而這些欄位的命名參數和值在類型名稱后面的括弧內指定。 參數通常為可選參數，並且經常影響篩選、格式化或轉換查詢結果的方式。

您傳遞 `id` 引數 `country`，指定要查詢的特定國家/地區，以及 `filters` 引數 `categories`.

## 欄位一直往下

你可能會想 `country` 和 `categories` 作為單獨的查詢或實體，在查詢中表示的整個樹實際上只包含欄位。 的運算式 `products` 在語法上與 `categories`. 兩者都是田地，它們的構造之間沒有區別。

任何GraphQL資料圖表都有單一「根」類型(通常稱為 `Query`)來啟動樹，且通常被視為實體的類型會指派給此根上的欄位。 範例查詢實際上是對根類型進行一個一般查詢並選取欄位 `country` 和 `categories`. 接著，選取這些欄位的子欄位，依此類推，可能是深度數個層級。 只要某欄位的返回類型是複雜類型（例如，具有其自有欄位而非標量類型的欄位），請繼續選擇所需欄位。

巢狀欄位的這個概念也是您為何可以傳遞 `products` (`pageSize` 和 `currentPage`)，就像您為頂層 `categories` 欄位。

![GraphQL欄位樹](../assets/graphql-field-tree.png)

## 變數

讓我們嘗試不同的查詢：

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

首先要注意的是新增了關鍵字 `query` 在查詢的左大括弧前，連同操作名稱(`getProducts`)。 此操作名稱是任意的；它與伺服器架構中的任何項目不對應。 已新增此語法以支援引入變數。

在上一個查詢中，您直接以字串或整數形式將欄位引數的值加上硬式編碼。 但是，GraphQL規格支援使用變數將使用者輸入與主要查詢分開的第一類。

在新查詢中，您使用整個查詢的左括弧前的括弧來定義 `$search` 變數（變數一律使用貨幣符號首碼語法）。 是此變數提供給 `search` 引數 `products`.

查詢包含變數時，GraphQL要求會與查詢本身並列包含個別的JSON編碼值字典。 對於上述查詢，除了查詢內文外，您還可以傳送下列變數值JSON:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您針對Venia範例網站(而非您自己的Adobe Commerce例項)嘗試這些查詢，傳回的結果可能會是空的 `related_products`.

在您用於測試的任何GraphQL感知用戶端（例如Altair和GraphiQL）中，UI支援分別輸入查詢與變數JSON。

正如您所見，GraphQL查詢的實際HTTP要求包含「query: `{string}`「」在內文中，任何包含變數字典的要求都只會包含額外的「變數： `{json}`&quot;在同一個身體里， `{json}` 是包含變數值的JSON字串。

新查詢也使用 _片段_ (`productDetails`)以在多個位置重複使用相同的欄位選取。 [深入了解片段](https://graphql.org/learn/queries/#fragments){target="_blank"} 在GraphQL檔案中。

{{$include /help/_includes/graphql-rest-related-links.md}}
