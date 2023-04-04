---
title: 了解如何安裝Adobe Commerce 2.4.6的IO事件
description: 了解如何在Adobe Commerce 2.4.6中安裝IO事件所需的模組，以便在Adobe Developer App Builder中使用
landing-page-description: 了解如何安裝Adobe Commerce 2.4.6所需的數個模組。
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Commerce 2.4.6安裝

了解如何使用2.4.6版的撰寫器在Adobe Commerce中安裝數個新模組。其他檔案請參閱 [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 這段錄像是給誰的？

* 使用I/O事件熟悉Adobe Commerce和Adobe Developer App Builder的開發人員。

## 視訊內容 {#video-content}

* 為內部部署托管運行的命令
* 要為Adobe Commerce Cloud運行的命令
* Adobe Commerce Cloud yaml需要編輯

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## 有用的命令 {#useful-commands}

視您是使用自行托管環境或使用Adobe Commerce Cloud而定，有各種命令會稍有不同。

### 內部部署托管 {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
