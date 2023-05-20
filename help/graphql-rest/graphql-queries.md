---
title: 使用GraphQL執行查詢
description: 瞭解如何使用GraphQL在Adobe Commerce和 [!DNL Magento Open Source]。 這是GraphQL使用GET和POST電話的簡介。
landing-page-description: 瞭解如何使用GraphQL在Adobe Commerce和 [!DNL Magento Open Source]。 這是GraphQL使用GET和POST電話的簡介。
short-description: 瞭解如何使用GraphQL在Adobe Commerce和 [!DNL Magento Open Source]。 這是GraphQL使用GET和POST電話的簡介。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# GraphQL查詢

讓我們用一個完整的示例來深入探討GraphQL查詢語法。 (請記住，您可以親自嘗試https://venia.magento.com/graphql。)

觀察以下GraphQL查詢，該查詢逐個分解：

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

GraphQL伺服器對上述查詢的可信響應可能是：

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

上例依賴於伺服器上定義的Adobe Commerce的開箱式GraphQL架構。 在此請求中，您一次查詢多種類型的資料。 查詢完全表示您需要的欄位，並且返回的資料的格式與查詢本身類似。

>[!NOTE]
>
>GraphQL客戶端對發送的實際HTTP請求的形式進行模糊處理，但這很容易發現。 如果使用基於瀏覽器的客戶端，請觀察 [!UICONTROL Network] 的子菜單。 您會看到該請求包含由「query: `{string}`」 `{string}` 只是整個查詢的原始字串。 如果請求作為GET發送，則可能會將查詢編碼在查詢字串參數「query」中。 與REST不同，HTTP請求類型不重要，只關係查詢的內容。


## 正在查詢所需內容

`country` 和 `categories` 在示例中，對於兩種不同的資料，表示兩種不同的「查詢」。 與傳統的API范式（如REST）不同，REST將為每個資料類型定義單獨和顯式端點。 GraphQL使您能夠靈活地使用一個表達式來查詢單個端點，該表達式可以同時提取多種類型的資料。

同樣，查詢會準確指定兩者所需的欄位 `country` (`id` 和 `full_name_english`) `categories` (`items`)，而您收到的資料將鏡像該欄位規範。 這些資料類型可能有更多欄位可用，但您只會返回您請求的內容。


>[!NOTE]
>
>您可能會注意到 `items` 實際上 _陣列_ 值，但您仍在直接為其選擇子欄位。 當欄位的類型是清單時，GraphQL會隱式地理解子選項，以應用於清單中的每個項。

## 參數

當要返回的欄位在每種類型的大括弧中指定時，它們的命名參數和值在類型名稱后的括弧中指定。 參數通常是可選的，並且通常會影響查詢結果的篩選、格式化或轉換方式。

您正在通過 `id` 參數 `country`，指定要查詢的特定國家/地區，以及 `filters` 參數 `categories`。

## 一路向下

你可能會想 `country` 和 `categories` 作為單獨的查詢或實體，在查詢中表示的整個樹實際上只包含欄位。 的表達式 `products` 在句法上和 `categories`。 都是田地，構造無異。

任何GraphQL資料圖形都具有單個「根」類型(通常指 `Query`)啟動樹，並且通常被視為實體的類型被分配給此根上的欄位。 該示例查詢實際上正在為根類型生成一個泛型查詢並選擇欄位 `country` 和 `categories`。 然後，選擇這些欄位的子欄位，等等，可能有幾個層次。 只要欄位的返回類型是複雜類型（例如，具有其自己欄位而不是標量類型的類型），請繼續選擇所需的欄位。

嵌套欄位的這一概念也是您為什麼可以為 `products` (`pageSize` 和 `currentPage`)就像你對頂層 `categories` 的子菜單。

![GraphQL田樹](../assets/graphql-field-tree.png)

## 變數

讓我們嘗試一個不同的查詢：

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

首先要注意的是添加了關鍵字 `query` 在查詢的左大括弧前，以及操作名稱(`getProducts`)。 此操作名稱是任意的；它與伺服器架構中的任何內容都不對應。 添加此語法是為了支援引入變數。

在上一個查詢中，您直接將欄位參數的硬編碼值作為字串或整數。 但是，GraphQL規範具有使用變數將用戶輸入與主查詢分離的一流支援。

在新查詢中，使用整個查詢的左括弧前的括弧來定義 `$search` 變數（變數始終使用美元符號前置詞語法）。 提供給 `search` 參數 `products`。

當查詢包含變數時，GraphQL請求應與查詢本身一起包含單獨的JSON編碼值字典。 對於上述查詢，除了查詢正文外，您還可以發送以下變數值的JSON:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您正在針對Venia示例站點而不是您自己的Adobe Commerce實例嘗試這些查詢，則返回的結果可能為空 `related_products`。

在您正用於測試的任何GraphQL感知客戶端（如Altair和GraphiQL）中，UI支援將變數JSON與查詢分開輸入。

正如您看到GraphQL查詢的實際HTTP請求包含「query: `{string}`在其正文中，任何包含變數字典的請求都只包括附加的「變數」： `{json}`&quot;在同一身體里 `{json}` 是包含變數值的JSON字串。

新查詢還使用 _片段_ (`productDetails`)以在多個位置重用同一欄位選擇。 [閱讀有關片段的詳細資訊](https://graphql.org/learn/queries/#fragments){target="_blank"} 在GraphQL檔案里。

{{$include /help/_includes/graphql-rest-related-links.md}}
