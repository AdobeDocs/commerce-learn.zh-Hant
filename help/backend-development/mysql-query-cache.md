---
title: 瞭解mysql查詢快取的方式
description: 有時mysql查詢會備份以等待鎖定。 本教學課程說明什麼是查詢快取，以及如果您遇到問題時的一些設定建議。
kt: 13690
doc-type: video
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: d62f409324eda53ca42cf2e9c9aa80655a1277d7
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 瞭解mysql查詢快取

瞭解什麼是MySQL查詢快取，以及它如何運作的一些基本瞭解。 瞭解如何透過在mysql慢速查詢記錄檔中發現「正在等待查詢快取鎖定」出現在大量中，以偵測mysql查詢快取的問題。

## 這部影片是給誰看的？

- 架構師
- 開發人員
- DevOps

## 視訊內容

- 瞭解查詢快取
- 如何透過發現「等待查詢快取鎖定」來偵測您的查詢快取設定是否可能是問題
- 瞭解SQL如何儲存並用於尋找相符的查詢快取
- 組態設定的部分提示

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## 有用的資源

- [一般MySQL准則](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=en){target="_blank"}
- [MySQL中的死結](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}
- [galera復寫和緩慢查詢](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html){target="_blank"}
