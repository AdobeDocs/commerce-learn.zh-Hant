---
title: 設定、部署和自訂內嵌Webhook
description: 瞭解如何設定和自訂內嵌webhook，以促進Commerce與第三方後台系統之間的通訊。
landing-page-description: 瞭解如何使用Commerce整合入門套件，透過內嵌webhook將Commerce與協力廠商後台系統整合。
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 設定、部署和自訂內嵌Webhook

瞭解內嵌webhook的設定和自訂，以將Commerce與協力廠商後台系統整合&#x200B;。 此影片說明webhook如何提供可公開使用的端點，以將來自協力廠商系統的訊息調整為Adobe IO事件API，以解決系統間事件通訊的限制。 程式涉及在`actions.config.yaml`檔案中設定webhook、在`app.config.yaml`檔案中啟用它，以及部署它以確保正常運作。

影片涵蓋修改webhook程式碼的步驟，以便將協力廠商事件轉譯為與整合的訂閱事件型別相容的格式。 它會討論新增`event-mapping.json`檔案以方便此翻譯，並強調在變更後重新部署執行階段動作的重要性&#x200B;。 影片也強調驗證和轉換傳入事件裝載的意義，以符合預期的結構描述，確保成功處理並與Commerce API整合，以建立客戶。

## 客群

* 想要設定內嵌webhook的開發人員
* 任何想要自訂事件翻譯程式碼的人
* 想要瞭解驗證和裝載管理之重要性的開發人員和架構師

## 視訊內容

* 設定和部署：影片強調在`actions.config.yaml`檔案中設定擷取webhook，並在`app.config.yaml`檔案中將其啟用的重要性。 此外，它強調在進行變更後重新部署專案的必要性，以確保webhook可正常運作。
* 自訂相容性：自訂webhook程式碼以將協力廠商事件轉譯為與整合的訂閱事件型別一致的格式至關重要。&#x200B;URL 此自訂功能可確保系統之間的順暢通訊以及成功的事件處理。
* 驗證實施：企業負責實施符合其需求的驗證機制，以防止在使用擷取webhook時發出未經授權的請求。 此步驟對於維護整合的安全性和完整性至關重要。
* 裝載驗證和轉換：驗證和轉換傳入的事件裝載以符合預期的結構描述，對於成功處理和與Commerce API整合至關重要&#x200B;。 只要適當地修剪和對應欄位，整合功能就能使用必要的資料有效地運作。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 程式碼範例

* [自訂內嵌webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [新增內嵌排程器](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
