---
title: 建立要在API Mesh中使用的多源GraphQL
description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解一些常見錯誤以及如何解決它們。
landing-page-description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解如何建立具有多個源的網格以及如何解決一些常見錯誤。
short-description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]。 瞭解如何建立具有多個源的網格以及如何解決一些常見錯誤。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 建立具有多個源的網格

此視頻幫助開發人員瞭解如何在API Mesh中為Adobe DeveloperApp Builder建立具有多個源的網格。 此視頻顯示如何建立具有多個源的網格並識別錯誤。 有關詳細資訊和代碼示例，請訪問 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}。

## 這段錄像是給誰的？

* 任何新加入API網格的人
* 有興趣合併多個API和GraphQL源的開發人員

## 視頻內容

* 如何使用 [變換](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} 修改預設源架構
* 如何解決錯誤，如名稱衝突、架構可用性和其他架構語法問題
* 使用修改的配置更新網格

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## 建立json配置檔案

API Mesh使用JSON配置檔案來定義源處理程式。 JSON檔案包含 `sources` 包含網格源的陣列。 這是一個具有多個源的網格的示例。

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
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
