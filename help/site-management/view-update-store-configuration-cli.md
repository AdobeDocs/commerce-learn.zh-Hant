---
title: 使用命令列檢視及設定管理員設定
description: 瞭解如何使用命令列檢視和設定管理員設定。
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# 使用命令列檢視及設定管理員設定

示範如何使用Commerce CLI檢視、設定和尋找設定值。 瞭解值的儲存位置以及預設值的來源。

## 這部影片是給誰看的？

* Adobe Commerce開發人員

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/3439981?captions=chi_hant&learn=on)

## 教學課程中使用的一些命令

將密碼安全性設定變更為建議：

`$ php bin/magento config:set admin/security/password_is_forced 0`

顯示銷售訂單自動複製功能的電子郵件地址

`$ php bin/magento config:show sales_email/order/copy_to`

針對在管理中有值的設定，顯示空白結果

`php bin/magento config:show trans_email/ident_sales/email`

## 教學課程中使用的Mysql查詢

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## 在何處尋找預設銷售電子郵件

如何尋找在程式碼基底中某處定義的設定值？
`grep -rnw vendor/magento/ -e 'sales@example.com'`

若要在終端機中檢視頁面並顯示行號`cat -n vendor/magento/module-email/etc/config.xml`

## 其他資源

* [命令列工具](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html?lang=zh-Hant){target="_blank"}
* [設定管理員安全性](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html?lang=zh-Hant){target="_blank"}
