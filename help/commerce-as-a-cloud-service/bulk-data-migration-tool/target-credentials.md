---
title: 大量資料移轉工具 — Target認證
description: 瞭解如何在.env檔案中設定目標執行個體URL、Adobe IMS憑證和CDMS設定，然後再執行大量資料移轉工具。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# 設定大量資料移轉工具的目標認證

在執行大量資料移轉工具之前，請先在`.env`檔案中設定目標執行個體URL、Adobe IMS憑證和CDMS設定。 請確定您的Adobe IMS URL、目標URL和CDMS主機都符合相同的環境層級 — 中繼或生產環境。

## 這部影片是給誰看的？

* 解決方案架構師
* DevOps工程師
* 後端開發人員

## 視訊內容

* 使用experience.adobe.com上執行個體資訊面板的值，在`.env`檔案中設定目標執行個體REST和GraphQL URL以及目標租使用者ID。
* 設定Adobe IMS URL以符合您的環境層（預備或生產）和區域。
* 在Adobe Developer Console中從&#x200B;**Project** > **OAuth伺服器對伺服器**&#x200B;擷取Adobe IMS使用者端ID和使用者端密碼。
* 複製目標組織ID，並設定CDMS主機、連線埠和本機伺服器設定以符合您的環境。

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)
