---
title: 建立可下載的產品
description: 瞭解如何使用REST API和Adobe Commerce管理員建立可下載的產品。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# 建立可下載的產品

瞭解如何使用REST API和Adobe Commerce管理員建立可下載的產品。

## 這部影片是給誰看的？

- 網站管理員
- 電子商務銷售商
- 新的Adobe Commerce開發人員，瞭解如何使用REST API在Adobe Commerce中建立產品

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/3453958?learn=on&captions=chi_hant)

## 允許下載的網域

您必須指定允許下載的網域。 網域會透過命令列新增到專案的`env.php`檔案。 `env.php`檔案詳細說明允許包含可下載內容的網域。 如果是在&#x200B;_之前，使用REST API_&#x200B;建立可下載的產品，並更新`php.env`檔案，則會發生錯誤：

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

若要設定網域，請連線到伺服器： `bin/magento downloadable:domains:add www.example.com`

完成之後，就會在&#x200B;_downloadable_domains_&#x200B;陣列中修改`env.php`。

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

現在已將網域新增到`env.php`，您可以在Adobe Commerce管理員中或使用REST API建立可下載的產品。

請參閱[組態參考](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hant#downloadable_domains)以瞭解更多資訊。

>[!IMPORTANT]
>在某些版本的Adobe Commerce上，當您在Adobe Commerce管理員中編輯產品時，可能會收到以下錯誤。 產品是使用REST API建立，但連結的下載專案價格為`null`。

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`。

若要修正此錯誤，請使用更新連結API： `POST V1/products/{sku}/downloadable-links.`

如需詳細資訊，請參閱[使用cURL更新產品下載連結](#update-downloadable-links)區段。

## 使用cURL建立可下載的產品（從遠端伺服器下載）

此範例說明當要下載的檔案不在相同伺服器上時，如何使用cURL建立可下載的產品。 如果檔案儲存在S3儲存貯體或其他數位資產管理器中，則此使用案例是典型行為。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## 使用cURL建立可下載的產品(從Commerce應用程式伺服器下載)

此範例示範當檔案儲存於與Adobe Commerce應用程式相同的伺服器時，如何使用cURL從Adobe Commerce管理員建立可下載的產品。

在此使用案例中，當管理目錄的管理員選擇`upload file`時，檔案會傳輸到`pub/media/downloadable/files/links/`目錄。  Automation會根據下列模式，建立檔案並將其移動到其各自的位置：

- 每個上傳的檔案會根據檔案名稱的前兩個字元儲存在資料夾中。
- 開始上傳時，Commerce應用程式會建立或使用現有資料夾來傳輸檔案。
- 下載檔案時，路徑的`link_file`區段會使用附加至`pub/media/downloadable/files/links/`目錄的路徑部分。

例如，如果上傳的檔案名為`download-example.zip`：

- 檔案已上傳至路徑`pub/media/downloadable/files/links/d/o/`。
如果子目錄`/d`和`/d/o`不存在，則會建立它們。

- 檔案的最終路徑為`/pub/media/downloadable/files/links/d/o/download-example.zip`。

- 此範例的`link_url`值為`d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## 使用curl取得產品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## 使用Postman更新產品 {#update-downloadable-links}

使用端點`rest/all/V1/products/{sku}/downloadable-links`
`SKU`是在建立產品時產生的產品ID。 舉例來說，在下列程式碼範例中，數字為39，但請確定數字已更新，可使用您網站的ID。 這會更新可下載產品的連結。

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## 使用CURL更新產品下載連結

當您使用cURL更新產品下載連結時，URL會包含正在更新之產品的SKU。  在下列程式碼範例中，SKU是`abcd12345`。 當您提交指令時，請變更值以符合您要更新之產品的SKU。

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## 其他資源

- [可下載的產品型別](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=zh-Hant){target="_blank"}
- [可下載的網域設定指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hant#downloadable_domains){target="_blank"}
- [Adobe Developer REST教學課程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
