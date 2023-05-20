---
title: 瞭解如何在Adobe Commerce使用條件事件
description: 瞭解如何使用要在Adobe Developer應用程式生成器中使用的條件事件。
landing-page-description: 瞭解如何使用Adobe Commerce條件事件。
short-description: 瞭解如何使用Adobe Commerce條件事件。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Adobe Commerce條件事件

瞭解可在Adobe CommerceApp Builder中使用的條件事件。 其他文檔，請參閱 [為Adobe Commerce安裝Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}。

## 這段錄像是給誰的？

* 使用I/O事件的Adobe Commerce和Adobe DeveloperApp Builder的新開發人員，需要建立AdobeApp Builder項目。

## 視頻內容 {#video-content}

* 瞭解條件事件
* 瞭解新XML檔案io_events.xml的正確用法
* 瞭解如何配置條件事件
* 定義用於條件事件的規則
* 瞭解如何在Commerce實例中註冊事件 `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806?quality=12&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
