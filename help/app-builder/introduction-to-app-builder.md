---
title: Adobe Systems 商務的進程外擴充性
description: 瞭解 Adobe Systems 應用程式產生器以及為什麼它是進程外擴充性的重要方面。
landing-page-description: 瞭解什麼是應用程式產生器，以及它如何協助 Adobe Systems 商務開發策略。
short-description: 瞭解什麼是App Builder以及它如何幫助實施Adobe Commerce開發戰略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 0%

---

# App Builder簡介

從歷史上看，Adobe Commerce開發一直採用流程擴展性。 進程中模型要求任何新代碼與升級、伺服器的PHP版本以及Commerce使用的許多其他基本伺服器應用程式和服務相容。 Adobe DeveloperApp Builder使用流程外擴展性來避免這些相容性問題。

## 用於Adobe Commerce的應用程式生成器 {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe DeveloperApp Builder是一個無伺服器擴展性的平台，用於整合和建立自定義體驗以擴展Adobe解決方案，現在它可用於Adobe Commerce。 使用App Builder，您可以構建安全且可擴展的應用程式，這些應用程式擴展了Commerce本機功能並與第三方解決方案整合。 作為開發人員，您現在可以利用Adobe Commerce的流程外擴展性，而這反過來又提供了即時和長期的好處。

App Builder提供了統一的第三方擴展框架，用於整合和建立擴展的自定義應用程式 [!DNL Adobe Commerce]。 由於此擴展性框架構建在Adobe的基礎架構上，因此開發人員可以構建定製的微服務，並擴展和整合 [!DNL Adobe Commerce] 跨其他Adobe解決方案和第三方整合。

App Builder為客戶提供了擴展 [!DNL Adobe Commerce] 在各種使用情形中：

* 中間件可擴充性 — 通過構建自定義連接器或利用一套預構建的整合將外部系統與Adobe應用程式連接起來。
* 核心服務可擴充性 — 通過擴展具有自定義功能和業務邏輯的預設行為來擴展核心應用程式功能。
* 消費者體驗擴充性-延長核心體驗，以支援業務需求或版本編號客戶特定的數位屬性、商店和後勤應用程式。

Adobe Systems 開發人員應用程式產生器是一個基於雲端的解決方案，表示它會自動縮放。 此服務也是全域分發，不管您的地理位置。

## 為何要深入瞭解應用程式產生器

由於 Adobe Systems 商務並非完全 SAAS 產品，您開發的程式碼可能增加複雜性和升級問題。 透過使用「進程外擴充性」（例如應用程式產生器），您可以為 Adobe Systems 商務商店提供自訂且獨特的功能，而無需處理過程中的方法。

其他好處包括：

* 分離功能可讓您更快啟動。
* 升級現在更容易。 自訂功能位於商務基本代碼之外，可防止升級時的相容性問題。
* 在商務外部移動功能和邏輯可釋放通常由進程內開發方法使用的資源。

## 建築 {#architecture}

Adobe DeveloperApp Builder不是現成解決方案，而是提供了一個通用、一致且標準化的開發平台，用於擴展Adobe雲解決方案，如Adobe Commerce，包括：

* Adobe Developer控制台用於定制微服務和擴展開發。 在訪問建立插件和整合所需的所有工具和API時，構建和管理項目。
* 開源工具、 SDK和庫，用於構建自定義擴展和整合。 使用React Spectrum(Adobe的UI工具包)為所有Adobe應用提供一個通用UI。
* 服務，如I/O運行時，用於在Adobe的無伺服器平台上托管基礎架構；I/O事件，用於基於事件的整合。 Adobe還為儲存資料和檔案提供現成支援。
* Adobe Experience Cloud，在此您提交擴展和整合以發佈到Experience Cloud組織中。系統管理員可以審核、管理和批准這些擴展。 發佈後，您的自定義App Builder擴展和工具可以與其他Adobe Experience Cloud應用一起使用。

下圖說明了在App Builder上構建的標準應用程式如何使用這些功能：

![建築](/help/assets/app-builder/app-builder-architecture.jpeg)

有關App Builder體系結構的詳細資訊，請參閱 [體系結構概述](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}。

## AmazonSales Channel擴展 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel 擴充功能仍處於開發階段，尚未正式發佈。  這些影片與教學課程旨在說明如何將 Adobe Systems 開發人員應用程式產生器用於實用案例。

以下課程說明如何使用應用程式產生器擴充功能，將 Adobe Systems 商務連線至 Amazon Sales Channel。

* [技術概述應用程式產生器](../app-builder/app-builder-technical-overview.md)
* [擴充性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能示範應用程式產生器](../app-builder/app-builder-functional-demonstration.md)

## 使用應用程式產生器開始使用 {#additional-resources}

通過閱讀以下部落格，可以找到包含初始設定的可組合商務策略的概述：

[App Builder如何幫助提高您的商業平台的業務靈活性](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

為幫助您開始使用App Builder,Adobe已建立了以下文檔：

* [應用程式生成器入門](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 繼續學習文檔 {#appbuilder-documentation}

App Builder為開發人員提供視頻和文檔，包括幫助開發您自己的自定義應用程式的指南和參考文檔：

* [App Builder文檔](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder視頻](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 試用一個示例應用程式 {#appbuilder-codesamples}

準備開始發展了嗎？ 以下連結包含示例應用程式以幫助您啟動：

* [Adobe Systems 開發人員網站上的應用程式產生器 Code 實驗室](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支援 {#support}

如需開發人員支援請求，請使用 [ 體驗聯盟論壇 ](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) {target="_blank"} 尋求協助。

{{$include /help/_includes/app-builder-related-links.md}}
