---
title: 瞭解如何使用命令列檢視和設定管理員設定
description: 示範如何使用命令列檢視和設定設定值
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: 9d4b01d383e5492ccc0cbd27636a49581e8ffb5b
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# 瞭解如何使用命令列檢視和設定管理員設定

示範如何使用Commerce CLI檢視、設定和尋找設定值。 瞭解值的儲存位置以及預設值的來源。

## 這部影片是給誰看的？

- Adobe Commerce開發人員

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

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

若要在終端機中檢視頁面並顯示行號 `cat -n vendor/magento/module-email/etc/config.xml`

## 其他資源

- [命令列工具](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [設定管理員安全性](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
