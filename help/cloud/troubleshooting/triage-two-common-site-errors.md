---
title: 診斷並修正一些常見的Commerce Cloud錯誤
description: 解決導致網站無法載入的兩個常見Adobe Cloud專案錯誤。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: 'https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 938d2364-5176-55ec-80f1-9415253e5e51
    internal-label: Site Management
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# 無法診斷和修正服務，且發生錯誤

瞭解如何分類及解決在Adobe Commerce Cloud專案中看到的兩個常見錯誤。  瞭解這些錯誤發生的方式和原因，以及解決這些錯誤的建議步驟。

## 此影片給誰看

* 開發人員與IT專業人員
* 系統管理員

## 視訊內容

* 診斷並解決儲存問題：
* 管理維護模式
* 有效的疑難排解提示

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
