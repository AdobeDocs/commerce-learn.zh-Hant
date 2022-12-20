---
title: GraphQL簡介
description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 對Adobe Commerce和使用GraphQLGET和POST呼叫 [!DNL Magento Open Source].
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 對Adobe Commerce和使用GraphQLGET和POST呼叫 [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 9dc530107470617f88992d8eb2ed9feb017a6530
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# GraphQL簡介

GraphQL已迅速成為功能強大的用戶端應用程式與後端對話的業界標準。 隨著平台在無頭式實作領域不斷擴充功能，這個主題對Adobe Commerce開發人員而言日益重要。

若您是GraphQL的新手，本節將介紹基本概念和使用方式。

## 什麼是GraphQL?

GraphQL是唯一API查詢語言的規格，以及回應該查詢語言提供資料的執行階段。

傳統Web API（如REST）對於來回傳遞資料的不同系統來說效果不錯，但對於Progressive Web Application等現代應用程式連結體驗而言，效能卻不如最佳。 在這樣的應用程式中， _相同_ 應用程式透過web API通訊。 REST等方案的已規劃方法通常在這種情況下無法提供適當的靈活性，因為需要快速提取許多類型的資料。

GraphQL可讓用戶端以表達式方式描述 _恰好_ 所需資料。 單個請求可查詢多種類型的資料，而不需要多個網路請求來擷取多種資料類型。 此外，僅包含所要求的類型和欄位（以直觀地反映查詢的格式），可保持低響應。

實作GraphQL規格的執行階段可以用任何語言建置。 Adobe Commerce和 [!DNL Magento Open Source] 使用
[graphql-php](https://webonyx.github.io/graphql-php/) PHP實施，並在其上建立自己的層。

[檢視完整的GraphQL檔案](https://graphql.org/learn)

## 使用GraphQL用戶端

您需要GUI GraphQL用戶端來測試程式碼範例和教學課程。 有數個選項：

* [阿爾泰爾](https://altairgraphql.dev/) 是專為GraphQL打造的絕佳且功能齊全的用戶端。 Adobe在逐步影片中使用Altair。
* 如果您不想安裝案頭應用程式，您的
   [鉻黃](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja)、Firefox或 [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa) 瀏覽器。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql) 是GraphQL IDE在GraphQL基金會中的實作。 這不是可安裝的工具，而是可用來自行建立介面的套件。
* 如果你已經熟悉 [Postman](https://www.postman.com/)，雖然GraphQL查詢的功能不如專屬GraphQL客戶，但仍能提供相當完善的支援。

在GraphQL用戶端中，您應將請求提交至URL路徑 `/graphql` 在Adobe Commerce上 [!DNL Magento Open Source] 例項。 如果您偏好將現有例項用於測試，則可使用Venia主題的示範(PWA Studio的實作範例): `https://venia.magento.com/graphql`

