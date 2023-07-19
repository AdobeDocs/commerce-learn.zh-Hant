---
title: 瞭解如何在mysql緩慢查詢記錄中找出緩慢查詢，以及為什麼Galera DB復寫設計方法可能是原因
description: Galera DB的設計方法可讓資料複製到次要資料庫的時間比主要資料庫長。 瞭解如何在mysql緩慢查詢記錄中尋找這些事件，以及在緩慢查詢記錄中看到專案的基本原因，以及將來如何防止它們。
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: ae85993d98fb8620b763a212e5a2d7e53596870f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# 瞭解Galera DB復寫和相關MySQL緩慢查詢

Galera群集提供效能與擴充能力。 在考慮次要資料庫時，請務必瞭解資料複製的方式與主要資料庫上的不同。 主要資料庫可以執行大量作業。 當所有次要資料庫都執行復寫時，它們會一次執行一個動作。 例如，如果您有67,000,000個專案在刪除中，則在次要資料庫上，每個專案一次都會發生一次。 檢閱Mysql緩慢查詢記錄時，您會發現此動作可能需要很長的時間。 由於次要資料庫一次執行一個專案，因此這些專案不會同步，而且可偵測到效能影響。

如果可能的話，解決方案是批次處理大型作業，以協助次要資料庫與主要資料庫保持同步。 藉由批次執行動作，可讓您及時執行動作，並將對效能的影響降至最低。

## 這部影片是給誰看的？

- 架構師
- 開發人員
- DevOps

## 視訊內容

- 對次要資料庫進行一般復寫
- 瞭解流量控制
- 在mysql緩慢查詢記錄檔中尋找執行緒編號
- 大量執行只會發生在主要播放器。 一次複製1次
- 將大型認可批次化，以協助復寫跟上主要的

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## 有用的資源

- [Galera叢集](https://galeracluster.com/)