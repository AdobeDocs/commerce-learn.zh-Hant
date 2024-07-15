---
title: 瞭解Percona Toolkit pt-query-digest的運作方式及其使用原因
description: 從緩慢、一般和二進位記錄檔中分析MySQL查詢。 它也可以從tcpdump分析「SHOW PROCESSLIST」和MySQL通訊協定資料的查詢。
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: 598bff1fd2cefdc449d5ae3431401aec1e796313
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

瞭解為何使用pt-query-digest和一些現實世界的範例來協助加深推理。

## 這部影片是給誰看的？

- 架構師
- 開發人員
- DevOps

## 視訊內容

- 瞭解pt-query-digest用法
- 瞭解此Percona Toolkit功能的優點和缺點
- 瞭解結果並瞭解應考慮哪些可能的效能步驟

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## 程式碼參考

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 有用的資源

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
- MySQL中的[死結](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}
