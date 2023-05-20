---
title: 在API網格中建立GraphQL單源網格
description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解如何建立具有一個源的網格。
landing-page-description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解如何建立具有一個源的網格。
short-description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解如何建立具有一個源的網格。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 使用單個源建立網格

此視頻幫助開發人員瞭解如何在Adobe DeveloperApp Builder的API Mesh中使用單個源建立網格。 要使此基本示例按預期工作，您需要一個可公開訪問的API或GraphQL終結點。 視頻還說明了如何建立簡單 `mesh.json` 與Commerce實例一起使用的檔案。 有關詳細資訊和代碼示例，請訪問 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}。

## 這段錄像是給誰的？

* 任何新加入API網格的人
* 有興趣合併多個GraphQL和API源的開發人員
* 任何需要知道如何過濾網路頁籤和由GraphQL過濾的人

## 視頻內容

* 使用API Mesh作為反向代理
* 從JSON配置檔案建立網格
* 訪問新建立的GraphQL終結點

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## 建立json配置檔案

API Mesh使用JSON配置檔案來定義源處理程式。 JSON檔案包含 `sources` 包含網格源的陣列。 這是一個單源網格的示例。

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
