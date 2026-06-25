---
title: 可撰寫目錄資料模型(CCDM)存在的原因
description: 瞭解CCDM如何使用目錄檢視和銷售服務，保留一個統一的目錄，讓店面接收經過篩選的產品、價格和規則。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 可撰寫目錄資料模型存在的原因

現代商務團隊經常跨&#x200B;**品牌**、**地區**、**經銷商**&#x200B;和&#x200B;**數位頻道**&#x200B;銷售。 當每個管道都有自己的目錄復本時，團隊花在協調SKU、價格和可用性上的時間會多於改善購物者體驗的時間。 **Adobe Commerce Optimizer**&#x200B;後面的&#x200B;**Adobe可撰寫目錄資料模型(CCDM)**&#x200B;設計來反轉該模式： SaaS層中的&#x200B;**一個統一目錄**，具有&#x200B;**目錄檢視**&#x200B;和&#x200B;**原則**，可決定每個店面或整合可看到的內容。

## 這部影片是給誰看的？

* 剛接觸Commerce且在設定目錄來源、檢視和原則之前需要業務內容的Adobe Commerce Optimizer解決方案架構師和開發人員

## 視訊內容

* 為何重複、通道特定的目錄帶來營運風險並減緩創新速度
* CCDM如何保持產品資料統一，同時仍支援多品牌和多區域案例
* 目錄檢視如何成為共用基本目錄與特定店面或對象之間的「鏡頭」
* Merchandising Services API會如何取用這些檢視，以便Headless體驗與設定的目錄保持一致

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## 孤立目錄的挑戰

當每個經銷商網站、區域店面或品牌屬性維護&#x200B;**自己的目錄資料庫**&#x200B;時，幾個問題會複合出現：

* **複製** — 相同的SKU、說明和媒體輸入多次。
* **漂移** — 價格更新、新屬性或停產專案會停留在單一管道，但不會停留在其他管道。
* **較慢的啟動** — 每個新的接觸點都會重複大量的資料工作，而不是重複使用單一產品記錄。

CCDM存在，因此產品資訊可以存放在其他系統擴充的&#x200B;**一個可撰寫目錄**&#x200B;中，而店面仍會收到&#x200B;**適合管道的**&#x200B;分類和價格。

## 可撰寫目錄資料模型變更的內容

在Adobe Commerce Optimizer中，產品資料是從一或多個&#x200B;**目錄來源** （例如地區設定，例如`en-US`，或上游系統，例如PIM或ERP）中&#x200B;**擷取到統一的基本目錄**。 該來源會提供原始屬性和值。

**目錄檢視**&#x200B;接著定義該整合資料為商務內容&#x200B;**組織和公開的方式**：哪些產品傳遞您的&#x200B;**原則**、哪些&#x200B;**價格簿**&#x200B;套用，以及哪些&#x200B;**目錄來源**&#x200B;支援檢視。 因此，相同的基礎記錄可以支援&#x200B;**許多投影**，例如每個經銷商、地區或品牌的個別檢視，而不需要複製每個網站的整個目錄。

這個分隔 — **資料來自** （目錄來源）與&#x200B;**資料呈現方式** （目錄檢視） — 是團隊採用CCDM而不是為每個管道維護平行目錄的核心原因。

## 目錄檢視作為店面鏡頭

如[銷售服務的目錄檢視](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}中所述，目錄檢視的行為類似於&#x200B;**鏡頭**：購物者只會看到檢視允許的產品、價格和規則，而&#x200B;**基本目錄**&#x200B;仍然是共用的記錄系統。 此模型直接與&#x200B;**銷售服務**&#x200B;配對，因此API使用者端會傳遞正確的檢視（及相關標題），並針對每個體驗接收一致的原則導向回應。

若要更深入瞭解這些元件如何配合端對端流程，請參閱開發人員逐步解說[為您的店面建立可撰寫的目錄](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}。

## 相關內容

* [瞭解目錄檢視](./learn-about-the-ccdm-feature-catalog-views.md)
* [銷售服務的目錄檢視](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [為您的店面建立可撰寫的目錄](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [開始使用Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

