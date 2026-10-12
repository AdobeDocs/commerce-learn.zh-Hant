---
title: 使用CLI重設管理員URI
description: 瞭解如何在Adobe Commerce Cloud CLI中重設管理員URI。 當管理員URL變更導致存取問題時，此方法相當實用。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00.000Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
TQID: 'https://experienceleague.adobe.com/OgyaTHVhQSRFApJPYwkzodSyAJcBjwk8kIrw4VBXltQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: c4af0798-d497-5e6b-8380-19812c26d00a
    internal-label: Console
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%
---
# 使用cli重設管理員URI

瞭解如何使用Adobe Commerce Cloud cli命令重設管理員URI。 如果管理員URL從管理員變更過，但發生錯誤，而且您無法再存取管理員，此功能會很有用。

>[!VIDEO](https://video.tv.adobe.com/v/3439703?captions=chi_hant&learn=on)

## 教學課程中使用的一些命令

將設定變更為預期的自訂管理路徑URL為0：

`$ php bin/magento config:set admin/url/use_custom 0`

將自訂管理路徑URL的設定變更為0

`$ php bin/magento config:set admin/url/use_custom_path 0`
