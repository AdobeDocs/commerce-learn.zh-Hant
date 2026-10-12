---
title: 大量資料移轉工具 — 資料庫認證
description: 瞭解如何在執行移轉工具之前，使用Magento Cloud CLI或專案ID在.my.cnf檔案中設定來源資料庫連線。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
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

>[!VIDEO](https://video.tv.adobe.com/v/3496166?captions=chi_hant&learn=on)
