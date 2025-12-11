---
title: 設定Adobe Commerce
description: 瞭解如何設定Adobe Commerce以允許在Adobe Developer App Builder中使用事件。
landing-page-description: 瞭解如何設定Adobe Commerce以使用Adobe Developer App Builder使用的事件機制。
short-description: 瞭解如何設定Adobe Commerce以使用Adobe Developer App Builder使用的事件機制。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 設定Adobe Commerce

瞭解如何設定Adobe Commerce以公開事件。 在[安裝適用於Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}找到其他檔案。

## 這部影片是給誰看的？

* 剛開始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的開發人員，需要建立Adobe App Builder專案。

## 視訊內容 {#video-content}

* 在Commerce管理員中設定Adobe I/O事件
* 在Commerce管理員中儲存私密金鑰
* 在Commerce管理員中儲存唯一識別碼
* 建立事件提供者

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
