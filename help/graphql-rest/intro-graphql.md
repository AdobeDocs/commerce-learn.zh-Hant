---
title: GraphQL簡介
description: 瞭解如何在Adobe Commerce使用GraphQL [!DNL Magento Open Source]。 使用GraphQLGET和POST呼籲Adobe Commerce [!DNL Magento Open Source]。
landing-page-description: 瞭解如何在Adobe Commerce使用GraphQL [!DNL Magento Open Source]。 使用GraphQLGET和POST呼籲Adobe Commerce [!DNL Magento Open Source]。
short-description: 瞭解如何在Adobe Commerce使用GraphQL [!DNL Magento Open Source]。 使用GraphQLGET和POST呼籲Adobe Commerce [!DNL Magento Open Source]。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# GraphQL簡介

GraphQL已迅速成為強大客戶端應用程式與後端溝通的行業標準。 隨著該平台在無頭實施領域不斷擴展其功能，它正成為Adobe Commerce開發人員日益關注的話題。

如果您是GraphQL新人，本部分將介紹基本概念和用法。

## 什麼是GraphQL?

GraphQL是唯一API查詢語言和運行時的規範，它響應該查詢語言提供資料。

傳統的Web API（如REST）對於來回傳遞資料的不同系統來說非常有效，但對於Progressive Web Application等現代應用連結體驗而言，它提供的效能低於峰值。 在此類應用中， _相同_ 應用程式通過web API通信。 REST等方案的已編製方法通常不能提供適當的靈活性，因為需要快速提取多種資料。

GraphQL允許客戶端以表達式 _正確_ 它需要的資料。 一個請求可以查詢多種類型，而不是需要多個網路請求來獲取多種資料類型。 並且，通過僅包括（以直觀地鏡像查詢的格式）所請求的類型和欄位來保持響應的精益。

實現GraphQL規範的運行時可以用任何語言生成。 Adobe Commerce [!DNL Magento Open Source] 使用
[圖形 — php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP實現，並在其上構建自己的層。

[查看完整的GraphQL文檔](https://graphql.org/learn){target="_blank"}

## 使用GraphQL客戶端

您需要GUIGraphQL客戶端來test代碼示例和教程。 有幾種選擇：

* [阿爾泰爾](https://altairgraphql.dev/){target="_blank"} 是專為GraphQL打造的優秀且功能齊全的客戶。 Adobe在瀏覽視頻中使用Altair。
* 如果不想安裝案頭應用程式，則您還有Altair擴展
   [鉻](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} 按鈕。
* [圖形QL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} 是GraphQLIDE在GraphQL基金會的實施。 這不是可安裝的工具，而是您可以自己構建介面的軟體包。
* 如果你已經熟悉 [Postman](https://www.postman.com/){target="_blank"}儘管它的特色並不像GraphQL客戶那樣全面，但它對GraphQL的查詢也有不錯的支援。

在您的GraphQL客戶端中，您應將請求提交到URL路徑 `/graphql` 在你的Adobe Commerce上 [!DNL Magento Open Source] 實例。 如果您希望將現有實例用於test，則可以使用Venia主題的演示(PWA Studio的示例實現): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
