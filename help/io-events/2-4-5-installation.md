---
title: 瞭解如何為Adobe Commerce2.4.5安裝IO事件
description: 瞭解如何在Adobe Commerce2.4.5中安裝IO事件所需的模組，以便在Adobe DeveloperApp Builder中使用
landing-page-description: 瞭解如何使用作曲家安裝Adobe Commerce2.4.5所需的幾個模組。
short-description: 瞭解如何使用作曲家安裝Adobe Commerce2.4.5所需的幾個模組。
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce2.4.5
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 0%

---

# Adobe Commerce2.4.5安裝

瞭解如何使用Composer 2.4.5版在Adobe Commerce安裝幾個新模組。這設定了在Adobe Commerce應用程式中使用的所需模組。 其他文檔，請參閱 [為Adobe Commerce安裝Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}。

## 這段錄像是給誰的？

* 使用I/O事件的Adobe Commerce和Adobe DeveloperApp Builder新開發人員

## 視頻內容 {#video-content}

* 使用合成器安裝所需模組
* 要為本地托管運行的命令
* 要為Adobe Commerce Cloud運行的命令
* Adobe Commerce Cloud山

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 有用的命令 {#useful-commands}

根據您是在自主托管環境還是使用Adobe Commerce Cloud，有各種命令稍有不同。

### 本地托管 {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce雲 {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
