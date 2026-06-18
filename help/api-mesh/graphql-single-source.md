---
title: 在API Mesh中建立GraphQL單一來源網格
description: 瞭解如何在Adobe Commerce和Adobe App Builder上使用API Mesh。 探索如何使用單一GraphQL來源建立網格並存取新端點。
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 使用單一來源建立網格

此影片可協助開發人員瞭解如何在Adobe Developer App Builder的API Mesh中，使用單一來源建立網格。 為了讓此基本範例能夠運作，您需要可公開存取的API或GraphQL端點。 影片也說明如何建立簡易的`mesh.json`檔案，以搭配您的Commerce執行個體使用。 如需詳細資訊和程式碼範例，請造訪[建立網格](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}。

## 這部影片是給誰看的？

* 任何不熟悉API Mesh的人
* 有意合併多個GraphQL和API來源的開發人員
* 需要瞭解如何依GraphQL篩選網路標籤和篩選的人

## 視訊內容

* 使用API網格作為反向Proxy
* 從JSON設定檔案建立網格
* 存取新建立的GraphQL端點

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## 建立JSON設定檔案

API Mesh使用JSON設定檔案來定義您的來源處理常式。 JSON檔案包含`sources`陣列，其中包含您的網格的來源。 下列是具有單一來源的網格範例。

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
