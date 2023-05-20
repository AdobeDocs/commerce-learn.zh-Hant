---
title: 配置Adobe Commerce
description: 瞭解如何配置Adobe Commerce以允許事件在Adobe Developer應用生成器中使用。
landing-page-description: 瞭解如何配置Adobe Commerce以使用事件機制供Adobe Developer應用生成器使用。
short-description: 瞭解如何配置Adobe Commerce以使用事件機制供Adobe Developer應用生成器使用。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 配置Adobe Commerce

瞭解如何配置Adobe Commerce以公開事件。 其他文檔，請參閱 [為Adobe Commerce安裝Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}。

## 這段錄像是給誰的？

* 使用I/O事件的Adobe Commerce和Adobe DeveloperApp Builder的新開發人員，需要建立AdobeApp Builder項目。

## 視頻內容 {#video-content}

* 在Commerce管理中配置Adobe I/O事件
* 在Commerce管理員中保存私鑰
* 將唯一標識符保存到Commerce管理員中
* 建立事件提供程式

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
