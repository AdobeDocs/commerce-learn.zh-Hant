---
title: 設定Adobe Commerce
description: 了解如何設定Adobe Commerce，以允許在Adobe Developer App Builder中使用事件。
landing-page-description: 了解如何設定Adobe Commerce以使用事件機制，供Adobe Developer App Builder使用。
short-description: Learn how to configure Adobe Commerce to use the event mechanism for consumption by Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# 設定Adobe Commerce

了解如何設定Adobe Commerce以公開事件。 其他檔案位於 [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 這段錄像是給誰的？

* 使用I/O事件且剛接觸Adobe Commerce和Adobe Developer App Builder的開發人員，需要建立Adobe應用程式建立器專案。

## 視訊內容 {#video-content}

* 在商務管理員中設定Adobe I/O事件
* 在商務管理員中儲存私密金鑰
* 在商務管理員中儲存唯一識別碼
* 建立事件提供程式

>[!VIDEO](https://video.tv.adobe.com/v/3415799)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
