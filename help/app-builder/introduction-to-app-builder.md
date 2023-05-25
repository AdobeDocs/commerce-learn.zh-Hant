---
title: Adobe Commerce的程式外擴充性
description: 瞭解AdobeApp Builder，以及它為何是程式外擴充性的重要方面。
landing-page-description: 瞭解什麼是App Builder以及它如何協助Adobe Commerce開發策略。
short-description: 瞭解什麼是App Builder以及它如何協助Adobe Commerce開發策略。
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

過去，Adobe Commerce開發一直使用程式內的擴充功能。 進程內模型需要任何新程式碼才能與升級、伺服器的PHP版本，以及Commerce使用的許多其他基本伺服器應用程式和服務相容。 Adobe Developer App Builder使用程式外擴充功能來避免這些相容性問題。

## 適用於Adobe Commerce的App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder是整合及建立自訂體驗以延伸Adobe解決方案的無伺服器擴充性平台，現在可供Adobe Commerce使用。 透過App Builder，您可以建立安全且可擴展的應用程式，這些應用程式可擴充Commerce原生功能並與協力廠商解決方案整合。 作為開發人員，您現在可以利用Adobe Commerce的流程外擴充功能，而這又可提供即期和長期好處。

App Builder提供統一的協力廠商擴充性架構，可整合及建立可擴充的自訂應用程式 [!DNL Adobe Commerce]. 由於此擴充性架構是以Adobe的基礎架構所建置，因此開發人員可以建置自訂微服務，以及擴充和整合 [!DNL Adobe Commerce] 跨其他Adobe解決方案和協力廠商整合。

App Builder為客戶提供擴充功能的方式 [!DNL Adobe Commerce] 在各種使用案例中：

* 中介軟體擴充性 — 藉由建立自訂聯結器或利用預先建立的整合套件，將外部系統與Adobe應用程式連線。
* 核心服務擴充性 — 透過自訂功能和商業邏輯擴充預設行為，進而擴充核心應用程式功能。
* 使用者體驗擴充性 — 擴充核心體驗以支援業務需求，或建立客戶專屬的數位財產、店面和後台應用程式。

Adobe Developer App Builder是雲端型解決方案，這表示會自動擴充。 此服務也分佈於全球，無論您身在何處，都能提供最佳效能。

## 為何要進一步瞭解App Builder

由於Adobe Commerce不是完整的SAAS產品，因此您開發的程式碼可能會增加複雜性和升級問題。 透過使用程式外擴充功能（例如App Builder），您可以為Adobe Commerce商店提供自訂、獨特的功能，而不需要程式內方法。

其他優點包括：

* 分離式功能可加快啟動時間。
* 升級現在更輕鬆。 自訂功能位於Commerce程式碼基底之外，這可以防止升級時的相容性問題。
* 將功能和邏輯移出Commerce可釋放通常由程式內開發方法使用的資源。

## 架構 {#architecture}

Adobe Developer App Builder不是現成可用的解決方案，而是提供通用、一致且標準化的開發平台，用於擴充Adobe Cloud解決方案(例如Adobe Commerce)，包括：

* 用於自訂微服務和擴充功能開發的Adobe Developer主控台。 在存取建立外掛程式和整合所需的所有工具和API時，建立和管理專案。
* 開放原始碼工具、SDK和程式庫，用於建置自訂擴充功能和整合。 使用React Spectrum (Adobe的UI工具組)，讓所有Adobe應用程式擁有一個通用UI。
* 服務，例如I/O Runtime，用於在Adobe的無伺服器平台上託管基礎結構，以及I/O事件，用於事件式整合。 Adobe也提供儲存資料和檔案的立即可用支援。
* Adobe Experience Cloud可讓您提交擴充功能和整合內容，以在Experience Cloud組織中發佈。系統管理員可以檢閱、管理和核准這些擴充功能。 發佈後，您的自訂App Builder擴充功能和工具即可與其他Adobe Experience Cloud應用程式搭配使用。

下圖說明在App Builder上建置的標準應用程式如何使用這些功能：

![架構](/help/assets/app-builder/app-builder-architecture.jpeg)

如需App Builder架構的詳細資訊，請參閱 [架構概述](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## AmazonSales Channel擴充功能 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>AmazonSales Channel擴充功能仍在開發中，尚未正式發行。  這些影片和教學課程旨在示範如何將Adobe Developer App Builder用於實際使用案例。

下列教學課程會示範如何使用App Builder擴充功能將Adobe Commerce連結至AmazonSales Channel。

* [技術概覽App Builder](../app-builder/app-builder-technical-overview.md)
* [擴充性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能示範App Builder](../app-builder/app-builder-functional-demonstration.md)

## 開始使用App Builder {#additional-resources}

可撰寫商務策略的概觀，包括初始設定，請參閱以下部落格：

[App Builder如何協助提升商務平台的商業靈敏度](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

為協助您開始使用App Builder，Adobe已建立下列檔案：

* [App Builder入門](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 繼續學習使用檔案 {#appbuilder-documentation}

App Builder為開發人員提供影片和檔案，包括指南和參考檔案，以協助您開發自己的自訂應用程式：

* [App Builder檔案](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder影片](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 嘗試其中一個範例應用程式 {#appbuilder-codesamples}

準備好開始開發了嗎？ 下列連結包含協助您入門的範例應用程式：

* [Adobe Developer網站上的App Builder程式碼實驗室](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支援 {#support}

如需開發人員支援請求，請使用 [Experience League論壇](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 以取得協助。

{{$include /help/_includes/app-builder-related-links.md}}
