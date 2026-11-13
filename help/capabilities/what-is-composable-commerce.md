---
title: Commerce與Adobe可組合
description: 瞭解使用Adobe平台選項、Edge Delivery Services店面、API Mesh、App Builder和Commerce開發MCP工具的可撰寫商務。
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# 可撰寫的商務

可撰寫商務是一種架構方法，透過API結合商務功能。 您可以針對店面體驗、目錄管理、產品探索、結帳和其他業務需求選擇並整合元件，而不需要每個功能都來自一個緊密結合的應用程式。

Headless商務將店面展示層與商務後端分開。 可撰寫商務可讓您選擇哪些功能要整合，哪些功能要獨立整合，藉此建立在該分離的基礎之上。 Headless店面本身無法獨立更換每個後端元件。

Adobe透過commerce平台選項、店面技術以及整合和開發工具支援此方法。 Adobe Commerce as a Cloud Service (ACCS)、Adobe Commerce Optimizer (ACO)、Edge Delivery Services、API Mesh、App Builder和Commerce開發MCP有不同的用途。 它們不是可互換的產品或必要的組合棧疊。

## 優點與折衷方案

組合式方法可協助您：

* **調整客戶體驗：**&#x200B;選擇店面元件，並整合符合您業務需求的服務。
* **逐步現代化：**&#x200B;取代選取的功能，同時保留其他系統及其資料。
* **獨立擴充與演化：**&#x200B;設計整合與擴充功能，使其變更不需要發行整個商務應用程式。
* **重複使用商務功能：**&#x200B;使用支援的平台功能和API，而不是重建每個商務功能。

這些優點取決於實作。 評估技術成熟度、傳遞成本、授權、安全性和資料隱私權。 定義API合約的所有權、資料同步化、驗證、效能以及跨服務的失敗。 模組化程度更高的架構也能增加整合與營運的複雜性。

## Adobe Commerce架構選項

首先，決定您是需要Managed Commerce平台，還是想根據現有的交易式後端打造最新的客戶體驗。

如需視訊比較，請參閱[Adobe可用的商務選項](./adobes-available-commerce-options.md)。

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service是受管理的SaaS商務平台。 Adobe可管理核心應用程式、基礎架構和更新。 其架構包括Commerce Foundation、Headless店面功能、銷售服務和開發人員工具。

使用支援的擴充點評估Managed Commerce基礎時，請使用此選項。 對於自訂商業邏輯，請規劃支援的程式外擴充性，而不是假設現有的PHP擴充功能可以部署而不做變更。 ACCS不支援Luma店面；請在適用的實作計畫中納入店面移轉。

請參閱[Adobe Commerce as a Cloud Service總覽](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/overview)。

### Adobe Commerce Optimizer

Adobe Commerce Optimizer支援目錄管理、產品探索、搜尋、建議和店面體驗，同時可讓您保留現有的商務後端。 目錄資料可能來自外部來源，而協力廠商系統仍可負責購物車和結帳生命週期。

當目標是面向客戶的現代化時，且不需要取代交易式後端時，請使用此選項。 明確地規劃目錄擷取以及購物車和結帳整合。 ACCS和ACO是獨立的架構選項，而不是所需的組合棧疊。 在為您的實作選擇服務之前，請驗證目前的整合支援。

檢視[Adobe Commerce Optimizer概觀](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview)。

### 現有的Adobe Commerce部署

雲端基礎結構及內部部署上的Adobe Commerce也可以逐步採用Headless店面及支援的API型整合。 視部署而定，您可以保留現有店面或採用支援的Headless店面。 將現有的商務功能保持在一起，可以減少整合工作，同時讓選定的服務個別改進。

功能可用性、擴充功能相容性、授權和設定需求會因產品和版本而異。 在選擇架構之前，請先比較[Adobe Commerce平台和服務](https://experienceleague.adobe.com/en/docs/commerce/information-center/explore-platforms-and-services/platform-and-services)。

## 由Edge Delivery Services提供支援的Commerce店面

由Edge Delivery Services提供支援的Commerce店面可提供店面體驗和內容傳送層。 其Headless架構可讓團隊開發獨立於商務基礎的前端。 檔案式製作和視覺化編輯支援內容更新，而店面元件則使用Commerce API進行購物功能。

區分負責目錄資料、定價、購物車操作、結帳和訂單的店面介面和系統。 Edge Delivery Services不會取代交易式商務後端。 在實施店面之前，確定您所選後端的支援服務和整合路徑。

請參閱[Commerce Storefront檔案](https://experienceleague.adobe.com/en/tools/commerce-storefront/)。

## 整合性與擴充性

API Mesh和App Builder有助於連線和擴展商務功能，而不需要每個自訂都嵌入核心商務應用程式中。

### API網格

Adobe Developer App Builder的API Mesh將多個Adobe、私人和第三方API來源結合為可查詢的GraphQL端點。 用它來撰寫前端體驗的資料，並協調對現有服務的存取。

API組成不會移除定義API合約、驗證、錯誤處理或交易商業邏輯所有權的需求。 在整合設計中應明確說明這些職責。

檢視[API Mesh總覽](https://developer.adobe.com/graphql-mesh-gateway/)。

### Adobe App Builder

Adobe App Builder為延伸Adobe解決方案並在Adobe基礎結構（包括Adobe I/O Runtime）上執行的應用程式提供平台。 您可以使用它來進行程式外的自訂，以及與CRM、ERP和PIM等系統的整合。

針對您使用的Commerce產品，選擇支援的擴充功能點、活動和Webhook。 與核心商務發行生命週期分開設計和測試應用程式。

檢視[App Builder概觀](https://developer.adobe.com/app-builder/docs/intro_and_overview/)。

## Commerce開發MCP與技能

Commerce開發MCP和技能支援AI輔助的App Builder應用程式開發和店面自訂。 在移至ACCS時，這些範本也有助於將現有的Adobe Commerce PHP擴充功能移轉至App Builder應用程式。

模型內容通訊協定(MCP)伺服器會將相容的AI編碼使用者端連線到開發工具。 Adobe的工具包括Commerce Developer MCP Server、App Builder MCP Server和Dropins AI Tools，以及Commerce的特定技能。

Commerce開發MCP是開發工具，而不是店面交付、購物車、結帳或訂單處理服務。 應用程式無法自動相容於所有Commerce產品。 檢閱產生的變更，並針對目標環境加以測試。

請參閱[Commerce開發MCP與技能](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools)。

## 規劃可撰寫的實作

1. 定義業務目標、客戶體驗、安全性需求，以及團隊可用的技能。
2. 選擇Commerce平台，並識別要保留或取代的現有系統。
3. 選取支援的店面，並定義簡報、銷售和交易服務之間的界限。
4. 針對目錄資訊、價格、存貨、客戶、購物車、結帳和訂單，建立資料所有權和API合約。
5. 計畫跨整合的驗證、資料同步、快取、監控和失敗處理。
6. 以漸進方式實作，並在每次發行前驗證功能、效能和安全性。

可組合的實作可在元件符合業務需求且能可靠合作時取得成功。 選擇您需要的功能，而非單純地新增服務，讓架構更加模組化。

## 相關資源

探索這些教學課程、影片、實施指南和播放清單，以規劃您的架構並建立商務整合。

### Commerce平台與店面

* [Adobe Commerce as a Cloud Service架構](../commerce-as-a-cloud-service/architecture.md)
* [可組合式店面與模組化架構](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Adobe Commerce Optimizer概觀](../adobe-commerce-optimizer/overview.md)
* [可撰寫目錄資料模型存在的原因](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [使用Edge Delivery Services的購物者體驗](../edge-delivery-services/shopper-experience.md)

### 整合與開發

* [使用App Builder實現程式外擴充功能](../app-builder/foundations/introduction-to-app-builder.md)
* [開始使用API Mesh](../api-mesh/getting-started-api-mesh.md)
* [建立具有多個來源的API網格](../api-mesh/graphql-multiple-source.md)
* [使用AI和MCP工具建立自訂運費](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### 架構與開發檔案

* [移轉至Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/migration/overview)
* [Adobe Commerce Optimizer整合](https://experienceleague.adobe.com/en/docs/commerce/optimizer/integrations-overview)
* [開始使用Commerce Storefront開發](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Commerce開發MCP和技能的最佳實務](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### 播放清單

* [開始使用Adobe App Builder和Adobe Commerce的API Mesh](https://experienceleague.adobe.com/en/playlists/commerce-get-started-app-builder-and-api-mesh)
