---
title: 瞭解MySQL查詢快取的運作方式
description: 瞭解MySQL查詢快取的運作方式、如何在緩慢的查詢記錄中找到「等待查詢快取鎖定」來偵測快取問題，以及設定提示。
doc-type: Technical Video
duration: 444
last-substantial-update: 2023-07-27T00:00:00.000Z
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13690
exl-id: 8d3b0ec2-e80c-4457-b924-69e8b8cedf03
TQID: 'https://experienceleague.adobe.com/W91-fJGZtgfpp03ZtYmSh97oNqmCFpF8AvkTYDaOB-g'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%
---
# 瞭解MySQL查詢快取

瞭解什麼是MySQL查詢快取，以及對其運作方式的一些基本瞭解。 瞭解如何在MySQL慢速查詢記錄檔中找到「等待查詢快取鎖定」，以偵測MySQL查詢快取問題。

## 目標對象

* 架構師
* 開發人員
* DevOps

## 視訊內容

* 瞭解查詢快取
* 如何透過找到「等待查詢快取鎖定」來偵測您的查詢快取設定是否為問題
* 瞭解SQL如何儲存並用於尋找相符的查詢快取
* 組態設定的部分提示

>[!VIDEO](https://video.tv.adobe.com/v/3423382?captions=chi_hant&learn=on)

## 有用的資源

* [一般MySQL准則](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql){target="_blank"}
* [galera復寫和緩慢查詢](https://experienceleague.adobe.com/zh-hant/docs/commerce-learn/tutorials/extensibility/backend-development/galera-db-slow-replication){target="_blank"}
