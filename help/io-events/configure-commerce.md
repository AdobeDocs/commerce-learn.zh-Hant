---
title: 設定Adobe Commerce
description: 瞭解如何設定Adobe Commerce以允許在Adobe Developer App Builder中使用事件。
landing-page-description: 瞭解如何設定Adobe Commerce以使用Adobe Developer App Builder使用的事件機制。
short-description: 瞭解如何設定Adobe Commerce以使用Adobe Developer App Builder使用的事件機制。
kt: 11889
doc-type: tutorial
duration: 299
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 151
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

>[!VIDEO](https://video.tv.adobe.com/v/3430615?captions=chi_hant&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

