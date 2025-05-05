---
title: 使用CLI重設管理員URI
description: 瞭解如何在Adobe Commerce Cloud CLI中重設管理員URI。 當管理員URL變更導致存取問題時，此方法相當實用。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# 使用cli重設管理員URI

瞭解如何使用Adobe Commerce Cloud cli命令重設管理員URI。 如果管理員URL從管理員變更過，但發生錯誤，而且您無法再存取管理員，此功能會很有用。

>[!VIDEO](https://video.tv.adobe.com/v/3439703/?learn=on&captions=chi_hant)

## 教學課程中使用的一些命令

將設定變更為預期的自訂管理路徑URL為0：

`$ php bin/magento config:set admin/url/use_custom 0`

將自訂管理路徑URL的設定變更為0

`$ php bin/magento config:set admin/url/use_custom_path 0`
