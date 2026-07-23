---
title: 瞭解如何安裝Adobe Commerce 2.4.6的IO事件
description: 瞭解如何在Adobe Commerce 2.4.6中安裝IO事件所需的模組，以用於Adobe Developer App Builder
jira: KT-11887
doc-type: Tutorial
duration: 136
last-substantial-update: 2023-02-22
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
TQID: https://experienceleague.adobe.com/19IVX54xo-RAJAOuXNIABdy11ExmBRAbWugi4eZ0cF0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 9f50b87d13f48b239d814783eb2c56319946cb29
workflow-type: tm+mt
source-wordcount: 141
ht-degree: 0%

---

# Adobe Commerce 2.4.6安裝

瞭解如何使用Composer for version 2.4.6在Adobe Commerce中安裝數個新模組。 在[安裝適用於Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}找到其他檔案。

## 目標對象

* 剛開始使用I/O事件來使用Adobe Commerce和Adobe Developer App Builder的開發人員。

## 視訊內容 {#video-content}

* 針對內部部署託管執行的命令
* 針對Adobe Commerce Cloud執行的命令
* Adobe Commerce Cloud Yaml必要的編輯

>[!VIDEO](https://video.tv.adobe.com/v/3430639?captions=chi_hant&learn=on)

## 有用的命令 {#useful-commands}

根據您是在自行託管的環境或使用Adobe Commerce Cloud，有些命令稍微不同。

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

