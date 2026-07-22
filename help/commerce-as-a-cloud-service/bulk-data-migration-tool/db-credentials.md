---
title: 大量資料移轉工具 — 資料庫認證
description: 瞭解如何在執行移轉工具之前，使用Magento Cloud CLI或專案ID在.my.cnf檔案中設定來源資料庫連線。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# 設定大量資料移轉工具的資料庫認證

在執行大量資料移轉工具之前，請先在`.my.cnf`檔案中設定來源資料庫連線。 根據您的來源環境是內部部署還是Adobe Commerce as a Cloud Service (PaaS)，步驟會有所不同。

## 這部影片是給誰看的？

* 解決方案架構師
* DevOps工程師
* 後端開發人員

## 視訊內容

* 將`.my.cnf.example`複製到`.my.cnf`，並為您的來源連線建立名為的新區段。
* 如果您的來源是Adobe Commerce as a Cloud Service (PaaS)，請在`.my.cnf`中設定專案ID。
* 使用Magento Cloud CLI通道命令來取得主機、使用者、密碼、連線埠和資料庫值。
* 如果您的來源是內部部署，請在執行工具之前確認主機和連線埠連線。

>[!VIDEO](https://video.tv.adobe.com/v/3496152?learn=on)
