---
title: 瞭解可撰寫目錄資料模型中的CCDM原則
description: 瞭解Adobe可撰寫目錄資料模型中的STATIC和TRIGGER原則如何控制跨目錄檢視的產品可見性，而不需要重建目錄。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 349
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Adobe可撰寫目錄資料模型中的原則

如果&#x200B;**目錄檢視**&#x200B;是塑造購物者從統一基礎目錄中所看到內容的鏡頭，則&#x200B;**政策**&#x200B;是該鏡頭的組成部分。 此教學課程說明原則是什麼、**STATIC**&#x200B;和&#x200B;**TRIGGER**&#x200B;原則如何在&#x200B;**Carvelo Automous**&#x200B;示範案例中搭配運作，以及為何更新原則會立即生效，而不需重新建立目錄。

## 這部影片是給誰看的？

* 在Adobe Commerce Optimizer上設定目錄檢視和銷售規則的Commerce解決方案架構師和開發人員

## 視訊內容

* 產品屬性上的資料存取篩選器原則
* 在Celport目錄檢視中，多個原則如何結合（品牌和零件類別）
* 永久商業規則的靜態原則
* 由API要求標頭啟用的觸發原則（例如`AC-Policy-Brand`）
* 在不重建目錄的情況下更新日常作業中的原則

>[!VIDEO](https://video.tv.adobe.com/v/3491413?learn=on)

**原則**&#x200B;是&#x200B;**資料存取篩選器**。 它會檢查產品屬性並套用規則，以決定目錄檢視可公開哪些產品。 原則位於共用可撰寫目錄的頂端，不會複製目錄資料。

## 靜態原則

**STATIC**&#x200B;原則是&#x200B;**永遠在**。 無論購物者行為或工作階段狀態為何，都會強制永久商業規則。

在Celport案例中，STATIC規則包括：

* Celport只會銷售&#x200B;**Bolt**&#x200B;和&#x200B;**Cruz**&#x200B;品牌。
* Celport只會顯示&#x200B;**個剎車**&#x200B;和&#x200B;**暫停**&#x200B;部分。

這些規則絕不會關閉。 靜態原則是&#x200B;**授權合約**、**地區限制**&#x200B;和&#x200B;**品牌許可權**&#x200B;的所在位置。 設定一次這些變數，Adobe Commerce Optimizer就會在每次請求時自動強制執行。

## 觸發原則

值為&#x200B;**TRIGGER**&#x200B;的原則有時稱為&#x200B;**專屬原則**。 只有在API呼叫的標頭中指定觸發程式&#x200B;**時，目錄檢視才會執行原則**。

例如，Experience Delivery Services (EDS)店面可能會提供包含&#x200B;**所有品牌**、**Aurora**、**Bolt**&#x200B;和&#x200B;**Cruz**&#x200B;的下拉式清單：

* 初始頁面檢視不會選取任何專案，因此不會傳送額外的品牌標頭。
* 當購物者選取&#x200B;**Bolt**&#x200B;時，基礎API會將`AC-Policy-Brand`標頭設定為`Bolt`。
* 當購物者選取&#x200B;**Cruz**&#x200B;時，相同的標頭會設定為`Cruz`。

目錄檢視只會在該標頭出現時套用觸發原則，這支援互動式篩選，而不會變更永久STATIC規則。

## 靜態與觸發器在一起

**靜態**&#x200B;原則處理永久規則。 **TRIGGER**&#x200B;原則處理互動原則。 它們在單一目錄檢視上搭配使用，可提供&#x200B;**法規遵循**&#x200B;和&#x200B;**彈性** — 固定的分類界限加上購物者導向的細分。

## 更新原則而不重建目錄

對於日常操作，原則變更是立即的。 假設Celport的授權合約現在允許&#x200B;**輪胎**&#x200B;以及剎車和暫停：

1. 開啟相關原則。
1. 將&#x200B;**tires**&#x200B;新增至值清單。
1. 儲存。

該變更為&#x200B;**立即上線**。 沒有目錄重建、重新部署或等待時間。 對Celport店面的下一個請求會反映此更新。

原則是&#x200B;**共用目錄**&#x200B;上的輕量型篩選器，而非製作成個別目錄復本的規則。 **變更規則，而非資料。**

## 相關內容

* [可撰寫目錄資料模型存在的原因](./why-ccdm-exists.md)
* [瞭解目錄檢視](./learn-about-the-ccdm-feature-catalog-views.md)
* [銷售服務的目錄檢視](https://experienceleague.adobe.com/zh-hant/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/zh-hant/docs/commerce/optimizer/overview){target="_blank"}
* [開始使用Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

