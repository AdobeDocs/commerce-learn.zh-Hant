---
title: 建立模組
description: 建立一個傳回json的頁面，並帶有一個引數。
topic: Development
kt: 5602
doc-type: video
activity: use
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 32d1366758fa6453a48570cfd08d10a93559a974
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 建立模組

模組是結構元素 [!DNL Commerce]  — 整個系統是以模組為基礎。 通常，建立自訂化的第一步是建立模組。

## 這部影片是給誰看的？

- 開發人員

## 新增模組的步驟

- 建立模組資料夾。
- 建立etc/module.xml檔案。
- 建立registration.php檔案。
- 執行bin/magento設定。
- 升級指令碼以安裝新模組。
- 檢查模組是否正常運作。

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## 有用的資源

- [模組參考指南](https://developer.adobe.com/commerce/php/module-reference/)
