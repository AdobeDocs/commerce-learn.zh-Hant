---
title: 在API網格中建立GraphQL單一來源網格
description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有一個源的網格。
landing-page-description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有一個源的網格。
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has one source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 使用單個源建立網格

此影片可協助開發人員了解如何在Adobe Developer App Builder的API Mesh中使用單一來源建立網格。 為了讓此基本範例如運期般運作，您需要可公開存取的API或GraphQL端點。 影片也說明如何建立簡單 `mesh.json` 檔案來搭配您的商務例項使用。 如需詳細資訊和程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 這段錄像是給誰的？

* 任何剛接觸API Mesh的人
* 有興趣結合多個GraphQL和API來源的開發人員
* 需要了解如何篩選網路標籤及依GraphQL篩選的任何人

## 視訊內容

* 使用API Mesh作為反向代理
* 從JSON設定檔案建立網格
* 存取新建立的GraphQL端點

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## 建立json設定檔案

API網格使用JSON配置檔案來定義源處理程式。 JSON檔案包含 `sources` 陣列，包含網格的源。 以下是單源網格的示例。

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
