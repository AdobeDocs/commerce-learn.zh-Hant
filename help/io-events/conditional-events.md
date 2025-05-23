---
title: 瞭解如何在Adobe Commerce中使用條件式事件
description: 瞭解如何使用要在Adobe Developer App Builder中使用的條件式事件。
landing-page-description: 瞭解如何使用Adobe Commerce條件事件。
short-description: 瞭解如何使用Adobe Commerce條件事件。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: e02da0901c01871360bcc556666e310acd7c7224
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adobe Commerce條件事件

瞭解可用於Adobe Developer App Builder的Adobe Commerce條件事件。 在[安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}找到其他檔案。

## 這部影片是給誰看的？

* 剛開始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的開發人員，需要建立Adobe的App Builder專案。

## 視訊內容 {#video-content}

* 瞭解條件事件
* 瞭解新XML檔案io_events.xml的正確用法
* 瞭解如何設定條件式事件
* 定義用於條件事件的規則
* 瞭解如何在Commerce執行個體`app/etc/config.php`中註冊事件

>[!VIDEO](https://video.tv.adobe.com/v/3430658?quality=12&learn=on&captions=chi_hant)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
