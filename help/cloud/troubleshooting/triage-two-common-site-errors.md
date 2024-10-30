---
title: 診斷並修正幾個常見的Commerce Cloud錯誤
description: 解決兩個阻止網站載入的常見Adobe Cloud專案錯誤。
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---


# 無法診斷和修正服務，且發生錯誤

瞭解如何分類及解決Adobe Commerce Cloud專案中看到的兩個常見錯誤。  瞭解這些錯誤發生的方式和原因，以及解決這些錯誤的建議步驟。

## 此影片給誰看

- 開發人員與IT專業人員
- 系統管理員

## 視訊內容

- 診斷並解決儲存問題：
- 管理維護模式
- 有效的疑難排解提示

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## 視訊中使用的命令

尋找回應訊息中提及的例外記錄的最後5行。

```SHELL
 tail -n 5 ~/var/log/exception.log
```

檢查硬碟空間。 請留意行dev/mapper/xxxx

```SHELL
df -h
```

讓我們找出前15個最大的檔案

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

顯示維護模式的狀態

```SHELL
php bin/magento maintenance:status
```

停用維護模式

```SHELL
php bin/magento maintenance:disable 
```
