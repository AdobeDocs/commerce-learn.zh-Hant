---
title: 瞭解Percona Toolkit pt-query-digest的運作方式及其使用原因
description: 從緩慢、一般和二進位記錄檔中分析MySQL查詢。 它也可以從tcpdump分析「SHOW PROCESSLIST」和MySQL通訊協定資料的查詢。
kt: 13846
doc-type: video
duration: 510
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
TQID: https://experienceleague.adobe.com/lh-fBjlhZO6W-K08KNb-KaG-N2slLZVpNOSg6LAp0n8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 113
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

瞭解為何使用pt-query-digest和一些現實世界的範例來協助加深推理。

## 這部影片是給誰看的？

* 架構師
* 開發人員
* DevOps

## 視訊內容

* 瞭解pt-query-digest用法
* 瞭解此Percona Toolkit功能的優點和缺點
* 瞭解結果並瞭解應考慮哪些可能的效能步驟

>[!VIDEO](https://video.tv.adobe.com/v/3452305?captions=chi_hant&learn=on)

## 程式碼參考

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 有用的資源

* [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
