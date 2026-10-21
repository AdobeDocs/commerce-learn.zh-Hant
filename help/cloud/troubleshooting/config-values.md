---
title: 設定值
description: 瞭解如何使用core_config_data、XML檔案和管理員設定，在Adobe Commerce中尋找、驗證和管理設定值。
feature: Cloud, Configuration, System, Variables
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner
doc-type: Technical Video
duration: 709
last-substantial-update: 2024-11-08T00:00:00.000Z
jira: KT-16429
exl-id: 9ff16e96-a63f-4fab-be7d-9160c1172603
TQID: 'https://experienceleague.adobe.com/-DrHhEXwmrrLBb63QKAYRdKbWQEFmJZ4uUhJa5ORFEs'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 8d0b446f-5b16-5a10-b272-01143504a11c
    internal-label: System
subfeature_v2:
  - id: 2191e157-828a-5358-ad69-ebcaa8402915
    internal-label: Variables
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%
---
# 設定值

本指南提供在Adobe Commerce中尋找、驗證和管理設定值的全面概觀。 它涵蓋了查詢core_config_data表格和使用CLI命令等基本方法，以及涉及XML檔案和環境特定配置的進階技術。 瞭解在不同環境中維持一致性的最佳實務，以及如何使用Adobe Commerce Cloud管理員UI和.magento.app.yaml等工具來設定環境變數。

## 此影片給誰看

* 使用Adobe Commerce和Adobe Commerce Cloud的開發人員
* 可能要求系統管理員或技術支援人員提供伺服器層級的協助

## 視訊內容

* 瞭解如何使用core_config_data表格、XML檔案和CLI命令在Adobe Commerce中尋找設定值。
* 瞭解如何透過管理面板、程式碼存放庫和環境特定檔案，編輯和覆寫設定值。
* 探索使用app:config:dump和Adobe Commerce Cloud管理UI等工具，在不同環境中維持一致組態設定的最佳做法。

>[!VIDEO](https://video.tv.adobe.com/v/3436458?learn=on)

## 相關Experience League檔案

* [匯出設定](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration)
* [覆寫組態設定](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/paths/override-config-settings)
* [設定設定值](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/set-configuration-values)
* [設定參考config.php](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/files/config-reference-configphp)
* [設定指南技術詳細資訊](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/deployment/technical-details)
* [鎖定的設定值](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/deployment/technical-details#:~:text=Configuration%20settings%20locked%20in%20the,php%20files)
* [在env.php](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin#:~:text=Cause,php%20)上儲存的值
