---
title: 建立新頁面
description: 瞭解如何在Adobe Commerce中建立新頁面，該頁面使用單一引數傳回JSON，包括模組設定、routes.xml設定和控制器動作。
jira: KT-5602
doc-type: Technical Video
duration: 259
feature: Page Content, Native Luma Frontend Development, Themes, Configuration
topic: Commerce, Development
role: Developer
level: Beginner
exl-id: aa830d15-0095-450f-83a8-a4ea489d6aae
TQID: 'https://experienceleague.adobe.com/WtDUQ2sH27ci33UMLBtuNac1oo2CVfBlnFdAbwM3dmY'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: 17d326fa-534a-55a5-b46f-8ae1de1e2f75
    internal-label: Page Content
  - id: 8b440f30-6794-5ed6-981f-391de4e9b0cc
    internal-label: Themes
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: aa64eb87-f03e-49eb-a367-66fa1adc2192
    internal-label: Native Luma Frontend Development
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%
---
# 建立新頁面

{{only-for-on-prem-commerce-cloud}}

建立一個傳回JSON的頁面，並包含一個引數。

## 這部影片是給誰看的？

* 開發人員

## 新增頁面的步驟

* 建立模組
* 新增&#x200B;**routes.xml**&#x200B;檔案
* 新增控制器（動作）檔案

## 建立模組的步驟

* 建立模組資料夾
* 建立`etc/module.xml`檔案
* 建立`registration.php`檔案
* 若要安裝新模組，請執行`bin/magento setup:upgrade`指令碼
* 驗證模組是否正常運作

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/35816?learn=on)

## 有用的資源

[前端開發人員指南](https://developer.adobe.com/commerce/frontend-core/guide/)

