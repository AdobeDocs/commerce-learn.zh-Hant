---
title: 瞭解如何安裝Adobe Commerce 2.4.6的IO事件
description: 瞭解如何在Adobe Commerce 2.4.6中安裝IO事件所需的模組，以用於Adobe Developer App Builder
landing-page-description: 瞭解如何安裝Adobe Commerce 2.4.6所需的數個模組。
short-description: 瞭解如何安裝Adobe Commerce 2.4.6所需的數個模組。
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Adobe Commerce 2.4.6安裝

瞭解如何使用Composer for version 2.4.6在Adobe Commerce中安裝數個新模組。在[安裝適用於Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}找到其他檔案。

## 這部影片是給誰看的？

* 剛開始使用I/O事件來使用Adobe Commerce和Adobe Developer App Builder的開發人員。

## 視訊內容 {#video-content}

* 針對內部部署託管執行的命令
* 針對Adobe Commerce Cloud執行的命令
* Adobe Commerce Cloud Yaml必要的編輯

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## 有用的命令 {#useful-commands}

有些命令稍微不同，這取決於您是在自行託管的環境或使用Adobe Commerce Cloud。

### 內部部署託管 {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### 雲端上的Adobe Commerce {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
