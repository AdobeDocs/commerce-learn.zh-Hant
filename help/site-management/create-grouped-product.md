---
title: 建立群組的產品
description: 瞭解如何使用REST API和商務管理員建立分組產品。
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: eec9a85198f963404f5ba82fc2fc76315a82f964
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# 建立群組的產品

群組產品由簡單獨立產品組成，並以群組呈現。 您可以提供單一產品的變數，或依季節或主題分組。 建立群組產品之前，請先確認要納入群組的所有簡單產品都可以在Adobe Commerce中使用，然後建立任何不存在的產品。 在本教學課程中，您將瞭解如何使用REST API和Adobe Commerce管理員建立分組產品。

使用REST API在「管理員」中建立群組產品：

1. 建立空白的分組產品。
1. 建立簡單產品以用於群組的產品。
1. 將簡單產品填入空白的分組產品中。
1. 建立空的群組產品並關聯簡單產品。

在Adobe Commerce管理員中建立分組產品時，建議先建立簡單產品。 當您準備好建立群組產品時，請將簡單產品指派給一個批次中的群組產品，以建立簡易產品的關聯。

承載中的排序順序屬性為必要項，且前端用來以所需順序顯示相關產品。

## 這部影片是給誰看的？

- 網站管理員
- 電子商務銷售商
- 新的Adobe Commerce開發人員，瞭解如何使用REST API在Adobe Commerce中建立分組的產品。

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## 群組產品的設定

在此範例中，有三個簡單產品（先建立）和一個群組產品。 兩個簡單產品與分組產品相關聯，然後將第三個簡單產品新增到分組產品中。

## 使用cURL建立第一個簡單產品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## 使用cURL建立第二個簡單產品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## 使用cURL建立第三個簡單產品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## 使用cURL建立空白的分組產品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## 將第一和第二個簡單產品新增到分組產品中

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## 將第三個簡單產品新增到現有的分組產品

包含適當的職位編號(除了 `1` 或 `2`)，用於最初與分組產品相關聯的前兩個產品。 在此範例中，位置為 `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## 從分組產品中刪除簡單產品

至 [刪除簡單產品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) 在群組的產品中，使用： `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

若要探索要當做何物使用 `{type}`，使用xdebug擷取要求並評估$linkTypes： `related`， `crosssell`， `uupsell`、和 `associated`.
![群組產品連結型別 — 替代文字](/help/assets/site-management/catalog/grouped-types.png "在xdebug工作階段期間擷取的分組產品連結型別")

將簡單產品連結至分組產品時，裝載包含幾個類似於的區段：

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

在承載中， `link_type` 值 `associated` 提供 `{type}` DELETE請求中所需的值。 請求URL將類似於 `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

請參閱cURL請求，刪除具有的簡單產品 `product-sku-three` 來自具群組之產品的SKU `my-new-grouped-product` SKU：

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## 使用cURL取得分組產品

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 其他資源

- [建立及管理群組的產品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [已分組的產品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
- [Adobe Developer其餘教學課程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
