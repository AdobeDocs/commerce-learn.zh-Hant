---
title: 建立要在API Mesh中使用的多來源GraphQL
description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解一些常見錯誤及解決方法。
landing-page-description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]. 了解如何建立具有多個來源的請求，以及如何解決一些常見錯誤。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 建立多來源GraphQL API網格

影片可協助開發人員了解如何使用多個來源建立GraphQL反向代理。 此影片說明如何拼接不同來源、識別錯誤並儲存變更至git。 若為影片中使用的基本程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 這段錄像是給誰的？

* 任何剛接觸API Mesh的人
* 有興趣使用多個圖形源的開發人員
* 需要知道如何按graphql篩選網路頁簽和篩選的任何人

## 視訊內容

* 複雜的自訂屬性API結構如何從第二個來源覆寫預設來源結構
* 修改api網狀配置以考慮第二個覆蓋架構
* 如何診斷在命名衝突、方案可用性和其他SDL語法等過程中可能發生的錯誤
* 嘗試匯整結構後常見錯誤的範例
* 在編輯後重建api網格
* 修改API Mesh設定後儲存對git的變更

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## 建立json設定檔案

若要讓Adobe應用程式產生器了解所有來源，請在JSON設定中定義。 每個來源都是陣列中的元素，您可以有一或多個。 以下是多個來源請求的範例，這些請求互相結合以形成單一回應。

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
        "name": "ERP",
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
