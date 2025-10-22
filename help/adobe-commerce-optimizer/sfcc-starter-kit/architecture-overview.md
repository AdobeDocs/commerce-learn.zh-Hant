---
title: Salesforce Commerce雲端聯結器應用程式的架構概觀
description: 瞭解使用Adobe Commerce Optimizer的Salesforce Commerce Cloud架構。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 243
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: fa615aab7b8eff3b13908797cd263ec4cdc65eb6
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# 瞭解Salesforce Commerce Cloud入門套件架構

瞭解Commerce Optimizer Connector Starter Kit的架構和功能，該套件整合了Salesforce Commerce Cloud (SFCC)、Adobe App Builder。 Adobe Commerce Optimizer使用入門套件來簡化Edge Delivery店面的目錄同步作業。 它說明了SFCC中的自訂卡匣如何透過差異匯出檔案來偵測目錄變更，並透過自訂API公開這些變更。 這些變更會由App Builder執行階段動作（同步和非同步）使用，以執行完整和差異同步、中繼資料更新和產品專屬同步。 此系統還包括驗證工具，以確保店面準確性，並使用App Builder的狀態管理來追蹤同步狀態並防止衝突。

## 這部影片是給誰看的？

* Commerce解決方案架構師
* 技術行銷工程師
* eCommerce Platform管理員

## 視訊內容

* 自訂SFCC卡匣和API會透過差異匯出偵測目錄變更，實現與Adobe App Builder的有效資料同步。
* App Builder執行階段動作會管理完整和差異同步、驗證和狀態追蹤，以確保Commerce Optimizer的更新準確且不會發生衝突。

>[!VIDEO](https://video.tv.adobe.com/v/3476062?captions=chi_hant&learn=on)
