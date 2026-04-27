---
title: 設定、部署和自訂內嵌Webhook
description: 瞭解如何設定和自訂內嵌webhook，以促進Commerce與第三方後台系統之間的通訊。
landing-page-description: 瞭解如何使用Commerce整合入門套件，透過內嵌webhook將Commerce與協力廠商後台系統整合。
kt: 15870
doc-type: video
duration: 697
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 432
ht-degree: 0%

---

# 設定、部署和自訂內嵌Webhook

瞭解整合Commerce與第三方後台系統的內嵌webhook的設定和自訂。&#x200B;此影片說明webhook如何提供公開可用的端點，以將來自第三方系統的訊息調整到Adobe IO Eventing API，解決系統之間的事件通訊限制。 程式涉及在`actions.config.yaml`檔案中設定webhook、在`app.config.yaml`檔案中啟用它，以及部署它以確保正常運作。

影片涵蓋修改webhook程式碼的步驟，以便將協力廠商事件轉譯為與整合的訂閱事件型別相容的格式。 影片討論新增`event-mapping.json`檔案以利此翻譯，並強調在變更後重新部署執行階段動作的重要性。影片也強調驗證和轉換傳入事件裝載&#x200B;以符合預期結構描述，以確保成功處理並與Commerce API整合，以建立客戶的重要性。

## 客群

* 想要設定內嵌webhook的開發人員
* 任何想要自訂事件翻譯程式碼的人
* 想要瞭解驗證和裝載管理之重要性的開發人員和架構師

## 視訊內容

* 設定和部署：影片強調在`actions.config.yaml`檔案中設定擷取webhook，並在`app.config.yaml`檔案中將其啟用的重要性。 此外，它強調在進行變更後重新部署專案的必要性，以確保webhook可正常運作。
* 自訂相容性：自訂webhook程式碼以將協力廠商事件轉譯為與整合的訂閱事件型別一致的格式至關重要。 此自訂&#x200B;功能可確保系統之間的順暢通訊以及成功的事件處理。
* 驗證實施：企業負責實施符合其需求的驗證機制，以防止在使用擷取webhook時發出未經授權的請求。 此步驟對於維護整合的安全性和完整性至關重要。
* 裝載驗證和轉換：驗證和轉換傳入的事件裝載以符合預期的結構描述，對於成功處理和與Commerce API整合至關重要。 只要適當地修剪和對應欄&#x200B;位，整合功能就能使用必要的資料有效運作。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 程式碼範例

* [自訂內嵌webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [新增內嵌排程器](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
