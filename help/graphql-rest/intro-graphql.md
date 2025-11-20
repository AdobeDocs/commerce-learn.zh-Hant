---
title: GraphQL簡介
description: 瞭解如何在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL。 對Adobe Commerce和 [!DNL Magento Open Source]使用GraphQL GET和POST呼叫。
short-description: 瞭解如何針對Adobe Commerce和 [!DNL Magento Open Source]使用GraphQL GET和POST呼叫。
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# GraphQL簡介

這是GraphQL和Adobe Commerce系列的第1部分。 GraphQL很快就成為強大的使用者端應用程式與後端通訊的業界標準。 隨著平台持續擴充其在Headless實作領域的功能，這對Adobe Commerce開發人員而言是一個日益相關的主題。

若您為GraphQL的新手，本節將引導您瞭解基本概念及使用方式。

>[!VIDEO](https://video.tv.adobe.com/v/3424117?learn=on)

## 本系列中GraphQL的相關影片和教學課程

* [第2部分GraphQL — 查詢](../graphql-rest/graphql-queries.md)
* [第3部分GraphQL — 變動](../graphql-rest/graphql-mutations.md)
* [第4部分GraphQL — 結構描述](../graphql-rest/graphql-schema.md)

## 什麼是GraphQL？

GraphQL是唯一API查詢語言和執行階段的規格，提供資料以回應該查詢語言。

REST等傳統網頁API對於不同系統間資料來回傳遞妥善運用，但對於Progressive Web Applications等現代應用程式連結體驗而言，其效能卻不及尖峰。 在像這樣的應用程式中，_same_&#x200B;應用程式的前端與後端層會透過網頁API通訊。 像REST這類架構的條理化方法通常無法在此情境下提供適當的彈性，因為許多型別的資料需要快速擷取。

GraphQL可讓使用者端以明確的方式描述&#x200B;_確切的_&#x200B;所需資料。 單一請求可以查詢多種型別，而不需要多個網路請求來擷取多種資料型別。 而且，回應會保持精簡狀態，只包含要求的型別和欄位（以直覺地反映查詢的格式）。

實作GraphQL規格的執行階段可以用任何語言建置。 Adobe Commerce和[!DNL Magento Open Source]使用
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP實作並在其上建置自己的圖層。

[檢視完整的GraphQL檔案](https://graphql.org/learn){target="_blank"}

## 使用GraphQL使用者端

您需要GUI GraphQL使用者端，以測試程式碼範例和教學課程。 有幾個選項：

* [Altair](https://altairgraphql.dev/){target="_blank"}是專為GraphQL打造的優秀且功能齊全的使用者端。 Adobe在逐步說明影片中使用Altair。
* 如果您不想安裝案頭應用程式，您也可以在的
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}、Firefox或[Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}瀏覽器。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"}是來自GraphQL Foundation的GraphQL IDE實作。 這不是可安裝的工具，而是您可用來自行建置介面的套件。
* 如果您已熟悉[Postman](https://www.postman.com/){target="_blank"}，雖然不如專用的GraphQL使用者端功能齊全，但已對GraphQL查詢提供良好的支援。

在您的GraphQL使用者端中，您應該將要求提交至您Adobe Commerce或`/graphql`執行個體上的URL路徑[!DNL Magento Open Source]。 如果您偏好使用現有執行個體進行測試，則可以使用Venia主題的示範(PWA Studio的範例實作)： `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
