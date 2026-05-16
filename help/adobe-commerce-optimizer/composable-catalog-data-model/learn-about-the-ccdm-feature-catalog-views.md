---
title: 瞭解可組合目錄資料模型中的目錄檢視
description: 瞭解Adobe Composable Catalog Data Model (CCDM)中的目錄檢視如何以不同的產品、價格和規則，將一個基本目錄對應至每個店面。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: 96a1356a399fa5cdca9d9befd7c14ebad1b0162f
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Adobe可撰寫目錄資料模型中的目錄檢視

目錄檢視是您以與單一可撰寫目錄不同的方式提供每個受眾的方式。 此教學課程說明目錄檢視是什麼、Adobe Commerce Optimizer如何將其套用至購物者，以及為什麼唯一檢視ID是產品、價格和規則的錨點 — 使用&#x200B;**Carvelo Automots**&#x200B;示範案例。

## 這部影片是給誰看的？

* 在Adobe Commerce Optimizer上模擬多品牌或多經銷商目錄的Commerce解決方案架構師和開發人員

## 視訊內容

* Carvelo汽車案例（品牌、經銷商及協定）
* 目錄檢視是什麼，以及統一基礎目錄上的「鏡頭」隱喻
* 店面如何使用目錄檢視來篩選產品和定價（例如Celport）
* 目錄檢視唯一ID和單一信任來源的商業價值

>[!VIDEO](https://video.tv.adobe.com/v/3491273?captions=chi_hant&learn=on)

## 案例：Carvelo Automous

**Carvelo Automobiles**&#x200B;是一家虛擬的汽車零件公司，通常用於Adobe Commerce示範、檔案和教學課程。 Carvelo透過下列三個經銷商銷售跨三種品牌的零件：**Aurora**、**Bolt**&#x200B;及&#x200B;**Cruz**：

* **Arkbridge**&#x200B;屬於West Coast Inc.。
* **Kingsbluff**&#x200B;和&#x200B;**Celport**&#x200B;屬於East Coast Inc.。

各經銷商對於可銷售的產品有各自的協定。 這樣會建立非常複雜的設定，而且仍然可以從&#x200B;**單一基底目錄**&#x200B;執行，包含大約600萬個SKU。 讓此功能成為&#x200B;**目錄檢視**。

## 什麼是目錄檢視？

**目錄檢視**&#x200B;是您針對特定商務內容所設定的目錄檢視。 將其視為&#x200B;**鏡頭**。 您的統一基礎目錄位於中間，儲存每個品牌的每個SKU。 每個經銷商都有自己的鏡頭 — 自己的目錄檢視。

在範例中：

* **Arkbridge**&#x200B;鏡頭可顯示所有三個品牌：Aurora、Bolt和Cruz元件。
* **Celport**&#x200B;鏡頭只顯示Bolt和Cruz零件的子集。

每個目錄檢視也可以控制&#x200B;**定價**。 **基礎目錄資料不會變更**，只會變更該內容的可檢視方面（分類、價格和規則）。

## Commerce Optimizer如何套用目錄檢視

當購物者造訪&#x200B;**Celport**&#x200B;店面時，Adobe Commerce Optimizer會使用&#x200B;**Celport目錄檢視**&#x200B;來確切判斷哪些產品、價格和規則適用。 購物者只會看到鏡頭允許的內容。

目錄中可能仍存在其他產品，例如Aurora輪胎、Bolt馬達或Cruz電池，但是&#x200B;**如果目錄檢視不允許的話，Celport的店面不會公開這些產品**。

## 目錄檢視ID和商業價值

每個目錄檢視都有&#x200B;**唯一識別碼**。 該ID可將您的店面連線到其目錄配置。 您在店面組態中設定一次此設定，而下遊行為會依循這些設定：正確的產品、正確的價格和正確的規則。

您不必為每個經銷商或品牌維護個別的目錄，也不必同步處理這些目錄，而是維護&#x200B;**一個**&#x200B;可撰寫的目錄。 目錄檢視是您塑造與該&#x200B;**單一信任來源**&#x200B;不同的&#x200B;**多個店面體驗**&#x200B;的方式。

## 相關內容

* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/zh-hant/docs/commerce/optimizer/overview){target="_blank"}
* [開始使用Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
