---
title: 將表格新增至資料庫
description: '"[!DNL Commerce] 有特殊機制，可讓您建立資料庫表格、修改現有表格，甚至新增部分資料。」'
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 將表格新增至資料庫

>[!IMPORTANT]
>
>不再建議使用此功能，請參閱https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] 擁有特殊的機制，可讓您建立資料庫表格、修改現有表格，甚至新增部分資料，例如安裝資料，這些資料必須在安裝模組時新增。 此機制可讓這些變更在不同安裝之間轉換。

開發人員不會重複在重新安裝系統時執行手動SQL作業，而是建立包含資料的安裝（或升級）指令碼。 指令碼會在每次安裝模組時執行。

## 這部影片是給誰看的？

- 開發人員

## 視訊內容

- 建立模組
- 建立InstallSchema指令碼
- 建立InstallData指令碼
- 新增模組並驗證已建立包含資料的表格
- 建立UpgradeSchema指令碼
- 建立UpgradeData指令碼
- 執行升級指令碼並確認表格已變更

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 有用的資源

- [將安裝/升級指令碼移轉至宣告式結構描述](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
