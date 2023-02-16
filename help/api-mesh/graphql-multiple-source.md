---
title: 建立要在API Mesh中使用的多來源GraphQL
description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解一些常見錯誤及解決方法。
landing-page-description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有多個來源的網格，以及如何解決一些常見錯誤。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b3d5b22a597b342df6bf9846346d656dd4ce1383
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# 使用多個源建立網格

此影片可協助開發人員了解如何在Adobe Developer App Builder的API Mesh中，使用多個來源建立網格。 此影片說明如何建立含有多個來源的網格並識別錯誤。 如需詳細資訊和程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 這段錄像是給誰的？

* 任何剛接觸API Mesh的人
* 有興趣結合多個API和GraphQL來源的開發人員

## 視訊內容

* 如何使用 [轉換](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/) 要修改預設源架構，請執行以下操作
* 如何疑難排解錯誤，例如名稱衝突、結構可用性和其他結構語法問題
* 使用修改的配置更新網格

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## 建立json設定檔案

API網格使用JSON配置檔案來定義源處理程式。 JSON檔案包含 `sources` 陣列，包含網格的源。 以下是多源網格的示例。

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
