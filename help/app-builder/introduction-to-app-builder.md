---
title: Adobe Commerce的程式外擴充性
description: 了解 Adobe App Builder，以及為什麼它對處理程序外擴充性很重要。
landing-page-description: 了解什麼是 App Builder，以及它如何協助制定 Adobe Commerce 開發策略。
short-description: 了解什麼是 App Builder，以及它如何協助制定 Adobe Commerce 開發策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 5%

---

# App Builder簡介

過去，Adobe Commerce開發一直使用程式內的擴充功能。 處理中模型需要任何新程式碼才能與升級、伺服器的PHP版本，以及Commerce使用的許多其他基本伺服器應用程式和服務相容。 Adobe Developer App Builder使用程式外擴充功能來避免這些相容性問題。

## 適用於Adobe Commerce的App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder是一個無伺服器擴充平台，可整合及建立自訂體驗來擴充Adobe解決方案，現在可供Adobe Commerce使用。 透過App Builder，您可以建立安全且具備擴充能力的應用程式，以擴充Commerce原生功能並與協力廠商解決方案整合。 作為開發人員，您現在可以利用Adobe Commerce的程式外擴充功能，而這又可提供即期和長期好處。

App Builder提供統一的第三方擴充架構，可整合及建立擴充的自訂應用程式 [!DNL Adobe Commerce]. 由於此擴充性架構是以Adobe的基礎建設為基礎，因此開發人員可以建立自訂微服務，並擴充及整合 [!DNL Adobe Commerce] 跨其他Adobe解決方案和協力廠商整合。

App Builder為客戶提供擴充功能的方式 [!DNL Adobe Commerce] 在各種使用案例中：

* 中介軟體擴充性 — 藉由建置自訂聯結器或利用預先建立的整合套件，將外部系統與Adobe應用程式連線。
* 核心服務擴充性 — 透過自訂功能和商業邏輯擴充預設行為，進而擴充核心應用程式功能。
* 使用者體驗擴充性 — 擴充核心體驗以支援業務需求，或建置客戶專屬的數位財產、店面和後台應用程式。

Adobe Developer App Builder是雲端型解決方案，這表示會自動縮放。 此服務也分佈於全球，無論您身在何處，都能提供最佳效能。

## 為何要進一步瞭解App Builder

由於Adobe Commerce不是完整的SAAS產品，因此您開發的程式碼可能會增加複雜性和升級問題。 透過使用程式外擴充功能（例如App Builder），您可以為Adobe Commerce商店提供自訂、獨特的功能，而不需要程式內方法。

其他優點包括：

* 分離式功能可加快上市時間。
* 升級現在更容易。 自訂功能位於Commerce程式碼基底之外，這可在升級時防止相容性問題。
* 在Commerce外部移動功能和邏輯會釋放通常由程式內開發方法使用的資源。

## 架構 {#architecture}

Adobe Developer App Builder並非開箱即用的解決方案，而是提供通用、一致且標準化的開發平台，以擴充Adobe Cloud解決方案(例如Adobe Commerce)，包括：

* 用於自訂微服務和擴充功能開發的Adobe Developer主控台。 在存取建立外掛程式和整合所需的所有工具和API時，建立和管理專案。
* 開放原始碼工具、SDK和程式庫，用於建立自訂擴充功能和整合。 使用React Spectrum (Adobe的UI工具組)，讓所有Adobe應用程式擁有一個通用UI。
* I/O Runtime之類的服務，可在Adobe的無伺服器平台上託管基礎結構，而I/O Events則可進行事件式整合。 Adobe也提供開箱即用的資料與檔案儲存支援。
* Adobe Experience Cloud可讓您提交擴充功能和整合內容，以在Experience Cloud組織中發佈。系統管理員可以檢閱、管理和核准這些擴充功能。 發佈後，您的自訂App Builder擴充功能和工具即可與其他Adobe Experience Cloud應用程式一併使用。

下圖說明在App Builder上建置的標準應用程式如何使用這些功能：

![架構](/help/assets/app-builder/app-builder-architecture.jpeg)

如需App Builder架構的詳細資訊，請參閱 [架構概述](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## AmazonSales Channel擴充功能 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>AmazonSales Channel擴充功能仍在開發中，尚未正式發行。  這些影片和教學課程旨在說明如何將Adobe Developer App Builder用於實際使用案例。

下列教學課程會示範如何使用App Builder擴充功能將Adobe Commerce連結至AmazonSales Channel。

* [App Builder技術概覽](../app-builder/app-builder-technical-overview.md)
* [擴充性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能示範App Builder](../app-builder/app-builder-functional-demonstration.md)

## 開始使用App Builder {#additional-resources}

您可以閱讀下列部落格，找到可撰寫的商務策略概觀，其中包括初始設定：

[App Builder如何協助提升您商務平台的業務靈敏度](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

為協助您開始使用App Builder，Adobe已建立下列檔案：

* [App Builder入門](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 繼續學習說明檔案 {#appbuilder-documentation}

App Builder為開發人員提供影片和檔案，包括指南和參考檔案，以協助開發您自己的自訂應用程式：

* [App Builder檔案](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder影片](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 試用其中一個範例應用程式 {#appbuilder-codesamples}

準備好開始開發了嗎？ 下列連結包含協助您開始使用的範例應用程式：

* [Adobe Developer網站上的App Builder程式碼實驗室](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支援 {#support}

如需開發人員支援請求，請使用 [Experience League論壇](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 以取得協助。

{{$include /help/_includes/app-builder-related-links.md}}
