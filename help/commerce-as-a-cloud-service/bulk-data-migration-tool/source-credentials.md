---
title: 大量資料移轉工具 — Source憑證
description: 瞭解如何在.env檔案中設定來源執行個體URL和驗證認證，然後再執行大量資料移轉工具。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
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
source-wordcount: '146'
ht-degree: 0%
---
# 設定大量資料移轉工具的來源認證

在執行大量資料移轉工具之前，請先在`.env`檔案中設定來源執行個體URL和驗證認證。 根據您的來源環境是內部部署還是Adobe Commerce as a Cloud Service (PaaS)，驗證步驟會略有不同。

## 這部影片是給誰看的？

* 解決方案架構師
* DevOps工程師
* 後端開發人員

## 視訊內容

* 在`.env`檔案中設定來源執行個體URL以及REST和GraphQL URL。
* 從Adobe Commerce管理中的&#x200B;**系統** > **擴充功能** > **整合**&#x200B;擷取或建立整合金鑰。
* 若要產生四個必要的權杖，請啟用整合。
* 如果您的來源為Adobe Commerce as a Cloud Service (PaaS)，請從account.magento.cloud擷取Magento CLI Token。

>[!VIDEO](https://video.tv.adobe.com/v/3496142?learn=on)
