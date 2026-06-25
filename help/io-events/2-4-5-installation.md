---
title: 瞭解如何安裝Adobe Commerce 2.4.5的IO事件
description: 瞭解如何在Adobe Commerce 2.4.5中安裝IO事件所需的模組，以用於Adobe Developer App Builder
landing-page-description: 瞭解如何使用撰寫器安裝Adobe Commerce 2.4.5所需的數個模組。
short-description: 瞭解如何使用撰寫器安裝Adobe Commerce 2.4.5所需的數個模組。
kt: 11886
doc-type: tutorial
duration: 214
audience: all
last-substantial-update: 2023-02-22T00:00:00.000Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
TQID: https://experienceleague.adobe.com/vb-q-JXeM4KvkxzfDui1MaTOSBFXDVBwmpTeolUtKGw
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 190
ht-degree: 0%

---

# Adobe Commerce 2.4.5安裝

瞭解如何使用Composer for version 2.4.5在Adobe Commerce中安裝數個新模組。 這會設定在Adobe Commerce應用程式中使用的必要模組。 在[安裝適用於Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}找到其他檔案。

## 這部影片是給誰看的？

* 不熟悉Adobe Commerce和Adobe Developer App Builder （使用I/O事件）的開發人員

## 視訊內容 {#video-content}

* 使用撰寫器安裝必要模組
* 針對內部部署託管執行的命令
* 針對Adobe Commerce Cloud執行的命令
* Adobe Commerce Cloud Yaml必要的編輯

>[!VIDEO](https://video.tv.adobe.com/v/3415794?learn=on)

## 有用的命令 {#useful-commands}

有些命令稍微不同，這取決於您是在自行託管的環境或使用Adobe Commerce Cloud。

### 內部部署託管 {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### 雲端上的Adobe Commerce {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

