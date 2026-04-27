---
title: Adobe如何建立可撰寫的Commerce
description: 瞭解可組合商務、排定API優先方法的優先順序，並實施模組化和服務導向的架構。
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 323
last-substantial-update: 2024-07-6
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: c32adafa-ed01-4b31-997e-2413013911b0id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: d095671a-1355-40aa-8b5f-06c33c68080bid: e0eb8757-182f-49f3-94a4-1587d16f5094id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1305
ht-degree: 0%

---

# 可撰寫的商務

可組合商務是電子商務的架構方法，涉及將前端展示層與後端商務功能分離。 它可讓&#x200B;企業選擇並組合最佳的元件或模組，以建立自訂的解決方案。 此方法包括將傳統整體電子商務平台分解為更小、更獨立的服務或微服務，以便共同組成。 可組合商務具備彈性、擴充性、自訂性、靈活性等優勢，並可更輕鬆地與其他系統和技術整合。

Adobe Commerce提供許多功能和工具，可支援商家採用和實施可組合商務。 Adobe Commerce提供可撰寫的商務方法以及混合式Headless和非Headless前端體驗。 Adobe秉持程式外擴充性的理念，提供API Mesh整合多項服務，以及Adobe App Builder建立自訂微服務。

## 可撰寫商務為何重要

由於多種原因，瞭解可組合商務對於參與電子商務產業的企業和個人很重要。 它提供彈性和客製化、擴充性和敏捷性、整合功能、經得起未來考驗，並賦予開發人員權力。 可組合商務可讓企業適應及發展電子商務平台、擴充及提升營運規模。 更令人印象深刻的好處包括能夠與其他系統和技術整合、跟上客戶不斷發展的期望，以及利用開發人員的專業知識。

它可協助企業瀏覽不斷演變的電子商務格局、做出明智的決策，並運用彈性、擴充性、自訂、敏捷性和整合功能等優勢。 此外，瞭解可組合商務對於業務成長、自訂、靈活性與創新、成本效益、整合與彈性，以及未來安全也很重要。 它讓企業有機會快速適應新功能，並因應不斷變化的市場環境，建立高度客製化的解決方案。 此外還有更多優點，包括快速創新、降低開發和維護成本，以及與協力廠商服務與系統整合的能力。

整體而言，瞭解可撰寫的商務可讓企業做出明智的決策、最佳化其電子商務架構和策略，並推動數位市場的增長和成功。

## 公司如何受益於可組合的商務

公司可以透過多種方式從可組合商務中受益：

**彈性與自訂：**&#x200B;可撰寫商務可讓公司選取並結合最佳的元件或微服務，以建立符合其特定需求的自訂電子商務解決方案。 它可讓企業量身打造平台，提供獨特的客戶體驗、實作專業化功能，並在市場上脫穎而出。

**擴充性與敏捷性：**&#x200B;透過可撰寫的架構，公司可以獨立擴充電子商務平台的不同元件。 This means they can allocate resources and scale specific functionalities based on demand, ensuring optimal performance and cost-efficiency. Composable commerce also enables agile development practices, allowing teams to work on different components simultaneously and deploy changes independently, resulting in faster time-to-market.

**Integration Capabilities:** Composable commerce facilitates seamless integration with third-party systems, services, and technologies. Companies can easily connect their e-commerce platform with various tools such as payment gateways, ERP systems, CRM systems, marketing automation platforms, and more. This integration capability enables businesses to leverage the best-of-breed solutions and create a unified ecosystem that enhances operational efficiency and customer experience.

**Future-Proofing:** Composable commerce provides companies with the flexibility to adapt and evolve their e-commerce platform as technology and market trends change. It allows businesses to easily add or replace components, integrate new technologies, and stay ahead of the competition. This future-proofing capability ensures that companies can continuously innovate and meet evolving customer expectations.

**Developer Empowerment:** Composable commerce empowers developers by providing them with the flexibility to work with different technologies, languages, and frameworks. It allows developers to focus on specific components or microservices, enabling specialization and expertise. This empowerment leads to increased developer productivity, faster development cycles, and the ability to leverage the latest tools and technologies.

Overall, composable commerce enables companies to create a highly flexible, scalable, and customizable e-commerce platform that can adapt to changing business needs, deliver exceptional customer experiences, and drive growth and success in the digital marketplace.

## What capabilities does Adobe Commerce have for composable commerce

Adobe Commerce provides several capabilities to support merchants in adopting and implementing composable commerce:

**Composable Commerce Methodology:** Adobe Commerce offers a composable commerce methodology that guides merchants in understanding and implementing the principles of composable architecture. This methodology helps businesses leverage the benefits of flexibility, scalability, and customization while considering factors like complexity, technical maturity, and project size.

**Feature-Rich Functionality:** Adobe Commerce provides a comprehensive set of features accessible through its GraphQLAPI. This feature-rich functionality allows merchants to minimize the number of vendors required in their commerce stack, reducing time-to-market challenges. It enables businesses to launch faster while maintaining the flexibility to compose and integrate additional third-party services or capabilities as their commerce stack evolves.

**Hybrid Front-End Experiences:** Adobe Commerce supports both headless and non-headless front-end experiences within the same ecosystem. This flexibility allows merchants to choose the most suitable architectural approach for each specific front-end use case. It enables a gradual transition to a headless and composable architecture without the need for a simultaneous migration of the entire system.

**API Mesh:** Adobe Commerce&#39;s API Mesh simplifies the integration of multiple microservices, third-party tools, and applications into a unified API endpoint for front-end developers. It allows developers to combine multiple data sources into a single GraphQL endpoint, reducing complexity and streamlining the development and maintenance of new features and experiences.

**Adobe App Builder:** Adobe App Builder is a serverless extensibility platform that allows merchants to create custom microservices, build custom experiences, and extend Adobe solutions. 透過App Builder，商家可以建立安全且可擴充的應用程式，擴充Adobe Commerce的原生功能並與協力廠商解決方案整合。 如此一來，商家就不需要建置和維護自訂與微服務的基礎架構，進而降低複雜度並降低總體擁有成本。

Adobe Commerce提供的這些功能簡化了可組合商務的採用和實作，讓商家能夠運用彈性、擴充性、自訂和整合功能的好處，同時降低複雜性和開發工作。

## 最終想法

可組合商務是電子商務的架構方法，涉及將前端展示層與後端商務功能分離。 以下是可撰寫商務的主要經驗教訓：

**架構：**&#x200B;可組合商務遵循模組化和分離架構，讓企業能夠選取並組合最佳元件或微服務，以建立自訂解決方案。 此架構提供彈性、擴充性和敏捷性。

**優點：**&#x200B;可組合商務提供數個優點，包括彈性與自訂、擴充性與靈活性、整合功能、可適應未來需求及增強開發人員能力。 它可讓企業適應、擴充、整合、創新及運用開發人員專業知識。

**考量事項：**&#x200B;在考慮可撰寫商務時，應仔細評估複雜性、內部技術成熟度、專案大小和結構、自訂與標準化、總體擁有成本，以及安全性和資料隱私權等因素。

**Adobe Commerce：** Adobe Commerce提供功能與工具，支援商家採用與實作可組合商務。 其中包括可撰寫的商務方法、功能豐富的功能、混合式前端體驗、整合的API Mesh以及用於自訂微服務的Adobe App Builder。

**業務影響：**&#x200B;可撰寫商務可讓企業建立高度彈性、可擴充且可自訂的電子商務平台。 它可讓客戶提供獨特的客戶體驗、根據需求擴充、與其他系統整合、經得起未來的營運，並運用開發人員的專業知識。

瞭解可組合商務對於電子商務產業的企業維持競爭力、適應不斷變化的市場環境及提供卓越的客戶體驗至關重要。 透過採用可組合商務，公司可充分運用彈性、擴充性、自訂性、靈活性及整合功能等優勢，推動數位市場的成長與成功。
