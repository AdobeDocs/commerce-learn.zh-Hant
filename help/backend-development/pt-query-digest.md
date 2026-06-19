---
title: 使用Percona Toolkit pt-query-digest分析MySQL查詢
description: 瞭解如何使用pt-query-digest從緩慢、一般和二進位記錄檔、SHOW PROCESSLIST和tcpdump的MySQL通訊協定資料中分析MySQL查詢。
doc-type: Technical Video
duration: 506
last-substantial-update: 2023-08-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13846
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
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 105
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

瞭解為何使用pt-query-digest和一些實用的範例來協助改善分析。

## 目標對象

* 架構師
* 開發人員
* DevOps

## 視訊內容

* 瞭解pt-query-digest用法
* 瞭解此Percona Toolkit功能的優點和缺點
* 瞭解結果並瞭解應考慮哪些可能的效能步驟

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## 程式碼參考

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 有用的資源

* [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
