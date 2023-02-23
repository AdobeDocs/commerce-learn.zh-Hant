---
title: 了解如何在Adobe Commerce中使用條件事件
description: 了解如何使用條件式事件來用於Adobe Developer App Builder。
landing-page-description: 了解如何使用Adobe Commerce條件事件。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: f5de9191315c7df2eaca4ee51d03b30e2a2fac99
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---


# Adobe Commerce條件事件

了解Adobe Commerce中可用於Adobe Developer App Builder的條件式事件。 其他檔案位於 [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## 這段錄像是給誰的？

* 使用I/O事件且剛接觸Adobe Commerce和Adobe Developer App Builder的開發人員，需要建立Adobe應用程式建立器專案。

## 視訊內容 {#video-content}

* 了解條件式事件
* 了解新XML檔案io_events.xml的正確用法
* 了解如何設定條件事件
* 定義用於條件事件的規則
* 了解如何在商務例項中註冊事件 `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
