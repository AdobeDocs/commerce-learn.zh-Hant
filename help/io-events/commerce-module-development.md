---
title: 建立Adobe Commerce模組以使用I/O事件
description: 瞭解如何建立Commerce模組以使用事件。
jira: KT-11891
doc-type: Tutorial
duration: 314
last-substantial-update: 2023-02-21
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
TQID: https://experienceleague.adobe.com/bRnOh6fnsyTY-21f81vIXV4-eeitLXQWsAbjg2rx-Is
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 140
ht-degree: 0%

---

# Adobe Commerce模組開發

瞭解如何註冊事件、尋找支援的事件，以及在自訂模組開發中使用新的XML檔案`io_events.xml`。 影片也會向開發人員展示如何尋找要使用的已註冊事件，以及如何移除已定義的事件。 在[安裝適用於Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}找到其他檔案。

## 這部影片是給誰看的？

* 剛開始使用I/O事件來使用Adobe Commerce和Adobe Developer App Builder的開發人員。

## 視訊內容 {#video-content}

* 為Adobe Developer App Builder註冊Commerce事件
* 識別可註冊的事件
* 瞭解如何在io_events.xml中註冊事件
* 瞭解如何在Commerce執行個體`app/etc/config.php`中註冊事件
* 瞭解如何取消訂閱事件

>[!VIDEO](https://video.tv.adobe.com/v/3415802?learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}


