---
title: Salesforce Commerce Cloud聯結器架構
description: 瞭解Salesforce Commerce Cloud Connector Starter Kit如何使用App Builder執行階段動作和差異匯出，將目錄與Adobe Commerce Optimizer同步。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00.000Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%
---
# Salesforce Commerce Cloud入門套件架構

瞭解Commerce Optimizer Connector Starter Kit的架構和功能，該套件整合了Salesforce Commerce Cloud (SFCC)和Adobe App Builder。 Adobe Commerce Optimizer使用入門套件來簡化Edge Delivery店面的目錄同步作業。 它說明了SFCC中的自訂卡匣如何透過差異匯出檔案來偵測目錄變更，並透過自訂API公開這些變更。 這些變更會由App Builder執行階段動作（同步和非同步）使用，以執行完整和差異同步、中繼資料更新和產品專屬同步。 此系統還包括驗證工具，以確保店面準確性，並使用App Builder的狀態管理來追蹤同步狀態並防止衝突。

## 這部影片是給誰看的？

* Commerce解決方案架構師
* 技術行銷工程師
* eCommerce Platform管理員

## 視訊內容

* 自訂SFCC卡匣和API會透過差異匯出偵測目錄變更，實現與Adobe App Builder的有效資料同步。
* App Builder執行階段動作會管理完整和差異同步、驗證和狀態追蹤，以確保Commerce Optimizer的更新準確且不會發生衝突。

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)

