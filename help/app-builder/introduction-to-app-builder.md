---
title: Adobe Commerce的流程外擴充性
description: 了解Adobe應用程式產生器，以及為何它是程式外擴充性的重要方面。
landing-page-description: 了解什麼是App Builder，以及它如何協助處理Adobe Commerce開發策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: f4c092b4534587f5656bbf298dbf94f783d93be7
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---


# App Builder簡介

過去，Adobe Commerce開發常使用程式內的擴充性。 進程中模型要求任何新代碼與升級、伺服器的PHP版本以及Commerce使用的許多其他基本伺服器應用程式和服務相容。 Adobe Developer App Builder使用程式外的擴充性，以避免這些相容性問題。

## Adobe Commerce適用的App Builder {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder是無伺服器的擴充性平台，可整合及建立自訂體驗以擴充Adobe解決方案，現在已適用於Adobe Commerce。 透過App Builder，您可以建立安全且可擴充的應用程式，以延伸Commerce原生功能並與協力廠商解決方案整合。 身為開發人員，您現在可以利用Adobe Commerce的流程外擴充功能，進而提供即時和長期的優點。

App Builder提供統一的協力廠商擴充性架構，可整合及建立可擴充的自訂應用程式 [!DNL Adobe Commerce]. 由於此可擴充性框架構建在Adobe的基礎架構上，因此開發人員可以構建定制微服務，並擴展和整合 [!DNL Adobe Commerce] 跨其他Adobe解決方案和協力廠商整合。

App Builder可讓客戶透過 [!DNL Adobe Commerce] 在各種使用案例中：

* 中間件的可擴充性 — 建立自訂連接器或運用預先建立的整合套件，將外部系統與Adobe應用程式連結。
* 核心服務的擴充性 — 透過自訂功能和業務邏輯擴充預設行為，以擴充核心應用程式功能。
* 使用者體驗的擴充性 — 擴充核心體驗以支援業務需求，或建立客戶專屬的數位屬性、店面和後台應用程式。

App Builder（先前稱為Project Firefly）是雲端式解決方案，這表示可自動縮放。 此服務也全球分發，不論您的地理位置為何，皆可提供最佳效能。

## 為何應深入了解App Builder

由於Adobe Commerce並非完全的SAAS產品，您開發的程式碼可能會增加複雜性並升級問題。 透過使用程式外的擴充性（例如App Builder），您可以為Adobe Commerce商店提供自訂、獨特的功能，而不需要程式內方法。

其他好處包括：

* 去耦功能可讓更短的啟動時間。
* 升級現在更輕鬆了。 自訂功能位於商務程式碼基底之外，可防止升級時出現相容性問題。
* 將功能和邏輯移至Commerce之外，可釋出通常用於程式內開發方法的資源。

## 架構 {#architecture}

Adobe Developer App Builder不提供現成可用的解決方案，而是提供通用、一致且標準化的開發平台，可擴充Adobe Commerce等Adobe雲端解決方案，包括：

* Adobe Developer主控台，用於自訂微服務和擴充功能開發。 建立及管理專案，同時存取建立外掛程式和整合所需的所有工具和API。
* 開放原始碼工具、SDK和程式庫，以建置自訂擴充功能和整合。 使用React Spectrum(Adobe的UI工具套件)來擁有適用於所有Adobe應用程式的通用UI。
* 諸如I/O Runtime等服務，用於在Adobe的無伺服器平台上托管基礎架構，以及I/O事件，用於事件型整合。 Adobe也提供儲存資料和檔案的現成可用支援。
* Adobe Experience Cloud，您可在此提交擴充功能和整合，以在Experience Cloud組織中發佈。系統管理員可以檢閱、管理及核准這些擴充功能。 發佈後，您的自訂App Builder擴充功能和工具可與其他Adobe Experience Cloud應用程式搭配使用。

下圖說明以App Builder為基礎的標準應用程式如何使用這些功能：

![架構](/help/assets/app-builder/firefly-architecture.jpeg)

如需App Builder架構的詳細資訊，請參閱 [架構概述](https://developer.adobe.com/app-builder/docs/guides/).

## AmazonSales Channel擴充功能 {#amazon-sales-channel-extension}

下列教學課程示範如何使用App Builder擴充功能將Adobe Commerce連線至AmazonSales Channel。

* [技術概述App Builder](../app-builder/app-builder-technical-overview.md)
* [可擴充性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能示範應用程式產生器](../app-builder/app-builder-functional-demonstration.md)

## 開始使用App Builder {#additional-resources}

為協助您開始使用App Builder,Adobe已建立下列檔案：

* [App Builder快速入門](https://developer.adobe.com/app-builder/docs/getting_started/)

## 繼續學習說明檔案 {#appbuilder-documentation}

App Builder提供開發人員的影片和檔案，包括指南和參考檔案，以協助開發您自己的自訂應用程式：

* [App Builder檔案](https://developer.adobe.com/app-builder/docs/overview/)
* [App Builder影片](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## 請試用其中一個示例應用程式 {#appbuilder-codesamples}

準備好開發了嗎？ 下列連結包含可協助您開始的範例應用程式：

* [Adobe Developer網站上的應用程式建立工具程式碼實驗室](https://developer.adobe.com/app-builder/docs/resources/)

## 支援 {#support}

若為開發人員支援請求，請使用 [Experience League論壇](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) 以求協助。

{{$include /help/_includes/app-builder-related-links.md}}
