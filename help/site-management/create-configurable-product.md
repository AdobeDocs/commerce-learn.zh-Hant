---
title: 建立可設定的產品
description: 瞭解如何使用REST API和Commerce管理員建立可設定的產品。
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# 建立可設定的產品

可設定的產品是多個簡單產品的父產品。 定義可設定的產品，要求採購員做出一或多個選擇來選取特定的產品變異。 例如，如果產品是襯衫，購買者必須選擇大小和顏色選項來選取襯衫。

雖然可設定的產品使用較多SKU，且一開始可能需要較長時間進行設定，但最終可為您節省時間。 如果您計畫拓展業務，可設定的產品型別對於具有多種選項的產品來說是個不錯的選擇。

建立可設定產品之前，請先確認可設定產品中可包含的所有簡單產品在Adobe Commerce中皆可使用。 建立任何不存在的專案。

在本教學課程中，瞭解如何使用REST API和Adobe Commerce管理員建立可設定的產品。

使用REST API建立可設定的產品：

1. 取得[屬性集](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html)的屬性，以使用後續API呼叫的ID號碼。
1. 建立簡單產品以用於可設定產品。
1. 建立空的可設定產品並關聯簡單產品。
1. 設定可設定產品的產品屬性。
1. 將簡單產品填入空的可設定產品中。
1. 取得可設定的產品和所有屬性。
1. 取得可設定產品的指派子產品。
1. 刪除簡單產品與可設定產品的關聯。

從Adobe Commerce管理員建立可設定的產品時，您可以先建立簡單產品，或使用自動化工具建立新的簡單產品，以便使用精靈。

## 這部影片是給誰看的？

- 網站管理員
- 電子商務銷售商
- 新的Adobe Commerce開發人員，瞭解如何使用REST API在Adobe Commerce中建立可設定產品

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## 使用cURL取得顏色屬性

在此範例中，會針對屬性集10傳回包含所有個別屬性的整個屬性集。 可能會很長，有數百行是很常見的事。 檢閱回應時，顏色的屬性ID可能會位於中央。 使用grep或其他方法搜尋結果，加快搜尋這些值。 我的回應接近第665行，並包含在以下JSON回應片段中。

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


若要擷取屬性ID以設定可設定的產品，請更新下列cURL請求的`attribute-sets/10/attributes`部分，以使用您環境中的屬性集ID取代`10`。 此要求使用GET方法。

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 使用cURL建立第一個簡單產品

### 調整環境ID和產品詳細資訊

使用API建立第一個簡單產品，以使用cURL傳送以下POST請求。

在提交請求之前，請使用您環境的值更新範例。

- 變更`"attribute-set": 10`以使用您環境的屬性集ID取代`10`。
- 變更`"value": "13"`以使用您環境的值取代`13`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## 使用cURL建立第二個簡單產品

使用API建立第二個簡單產品，透過cURL傳送以下POST請求。

在提交請求之前，請使用您環境的值更新範例。

- 變更`"attribute_set_id": 10,`並以您環境中的屬性集ID取代`10`。
- 變更`"value": "14"`並以您環境的值取代`14`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## 使用cURL建立第三個簡單產品

使用cURL傳送以下POST請求來建立第三個簡單產品。

在提交請求之前，請使用您環境的值更新範例。

- 變更`"attribute_set_id": 10,`以使用您環境的屬性集ID取代`10`。
- 變更`"value": "15"`並以您環境的值取代`15`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## 使用cURL建立空的可設定產品

使用cURL傳送以下POST請求，以建立空的可設定產品。

在提交請求之前，請使用您環境的值更新範例。

- 變更`"attribute_set_id": 10,`並以您環境的屬性集ID取代`10`。
- 變更`"value": "93"`並以您環境的值取代`93`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## 設定可供設定產品使用的選項

使用cURL傳送以下POST請求，設定可設定產品的可用選項。

在提交要求之前，請變更`"attribute_id": 93,`以使用您環境的屬性ID取代`93`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

如果您忘記設定可設定產品（上層）的選項，則嘗試將下層產品與可設定產品建立關聯時會出錯。 錯誤訊息類似於以下範例：

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 將子產品連結至可設定的

現在，您已建立三個簡單的產品：

- `"Kids Hawaiian Ukulele Red"`，
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

傳送下列POST請求，將這些簡單產品新增為可設定產品的子項。 針對每項產品提交個別請求。

對於每個請求，請以您要新增的子產品的值更新`childSKU`值。 下列範例將簡單產品`kids-Hawaiian-Ukulele-red`指派給具有SKU `Kids-Hawaiian-Ukulele-red`的可設定產品。


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## 使用cURL取得可設定的產品

現在您已建立可設定的產品並擁有三個指派的子SKU。 您可以使用cURL傳送下列GET請求，以檢視已指派產品的連結ID。 此請求會傳回有關可設定產品的詳細資訊。

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

以下專案使用GET方法

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 取得與可設定產品相關聯的子產品

透過傳送以下GET請求，僅傳回與可設定產品關聯的子項。 回應將包含子產品的所有屬性，包括SKU和價格。

以下專案使用GET方法

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 從父可設定產品中刪除或移除子產品

您可以使用cURL傳送以下DELETE請求，從可設定產品中移除子產品而不從目錄中刪除產品。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 其他資源

- [建立可設定的產品教學課程](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [可設定的產品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html){target="_blank"}
- [Adobe Developer REST教學課程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
