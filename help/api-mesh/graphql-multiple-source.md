---
title: 建立用於API Mesh的多重來源GraphQL
description: 瞭解如何在Adobe Commerce和 [!DNL Adobe App Builder]上為API Mesh使用多個來源。 瞭解一些常見錯誤以及如何解決這些錯誤。
jira: KT-11804
doc-type: Tutorial
duration: 409
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
TQID: https://experienceleague.adobe.com/O6ONn4NzMP-VqN0nsCoD-OPkZGMBelLWB-KNP1fZqmA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 199
ht-degree: 0%

---

# 建立具有多個來源的網格

此影片可協助開發人員瞭解如何在Adobe Developer App Builder的API Mesh中建立與多個來源的網格。 本影片說明如何建立具有多個來源的網格並識別錯誤。 如需詳細資訊和程式碼範例，請造訪[建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}。

## 這部影片是給誰看的？

* 不熟悉API Mesh的任何人
* 有意合併多個API和GraphQL來源的開發人員

## 視訊內容

* 如何使用[轉換](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"}來修改預設來源結構描述
* 如何疑難排解錯誤，例如名稱衝突、結構描述可用性和其他結構描述語法問題
* 使用修改的組態更新您的網格

>[!VIDEO](https://video.tv.adobe.com/v/3414125?learn=on)

## 建立json設定檔案

API Mesh使用JSON設定檔案來定義您的來源處理常式。 JSON檔案包含`sources`陣列，其中包含您的網格的來源。 以下是具有多個來源的網格範例。

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
