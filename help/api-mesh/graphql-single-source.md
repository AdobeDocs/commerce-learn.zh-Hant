---
title: 建立要在API Mesh中使用的GraphQL單一來源請求
description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有單一來源的請求。
landing-page-description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有單一來源的請求。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 建立單一來源GraphQL API網格

影片可協助開發人員了解如何建立GraphQL反向Proxy，且有單一來源。 請記住，GraphQL Mesh必須具備有效GraphQL架構，才能正常運作。 影片也說明如何設定您的初始json以搭配您的商務網站使用。 若為影片中使用的基本程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 這段錄像是給誰的？

* 任何剛接觸API Mesh的人
* 有興趣使用多個圖形源的開發人員
* 需要知道如何按graphql篩選網路頁簽和篩選的任何人

## 視訊內容

* 使用API做為反向代理
* 使用Adobe Developer命令列介面進行JSON設定
* 存取新建立的GraphQL端點

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## 建立json設定檔案

若要讓Adobe應用程式產生器了解所有來源，請在JSON設定中定義。 每個來源都是陣列中的元素，您可以有一或多個。 以下是單一來源的範例

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
