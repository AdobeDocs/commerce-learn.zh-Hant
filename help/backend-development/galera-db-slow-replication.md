---
title: 診斷MySQL中緩慢查詢記錄檔的Galera資料庫復寫
description: 瞭解Galera DB的複製設計如何減緩次要資料庫同步處理、如何在MySQL緩慢查詢記錄中識別這些事件，以及如何將影響降至最低。
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18T00:00:00.000Z
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: 'https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
subfeature_v2:
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
source-wordcount: '262'
ht-degree: 0%
---
# 瞭解Galera DB復寫和相關MySQL緩慢查詢

Galera叢集可提供效能與擴充能力。 在考慮復本資料庫時，請務必瞭解資料複製的方式與主要資料庫上的不同。 主要資料庫可以執行大量作業。 當針對所有復本資料庫進行復寫時，它們會一次執行一個動作。 例如，如果您有67,000,000個專案在刪除中，則復本資料庫上的每個專案一次都會發生一次。 檢閱MySQL緩慢查詢記錄時，您會發現此動作可能需要很長的時間。 復本資料庫會依序執行作業，這是不同步的原因，而且可以偵測效能影響。

為了協助復本資料庫與主要資料庫保持同步，請儘可能批次處理您的大型作業。 透過批次執行動作，可讓您及時執行動作，並將對效能的影響降至最低。

## 目標對象

* 架構師
* 開發人員
* DevOps

## 視訊內容

* 到復本資料庫的閘門復寫
* 瞭解流量控制
* 在mysql緩慢查詢記錄檔中尋找執行緒編號
* 大量執行只會發生在主要播放器。 一次複製1次
* 為協助復寫跟上主要專案的進度，請批次處理大型認可。

>[!VIDEO](https://video.tv.adobe.com/v/3423538?captions=chi_hant&learn=on)

## 有用的資源

* [Galera叢集](https://mariadb.com/products/enterprise/galera-cluster/)
