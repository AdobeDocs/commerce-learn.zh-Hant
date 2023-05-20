---
title: 向資料庫添加表
description: '"[!DNL Commerce] 有一種特殊機制，使您能夠建立資料庫表、修改現有表，甚至向其中添加一些資料。」'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: e8d2631b31319701beb327f42fdf1372d9dad9b7
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 向資料庫添加表

>[!IMPORTANT]
>
>不再建議這樣做，請參閱https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] 有一種特殊機制，使您能夠建立資料庫表、修改現有表，甚至向其中添加一些資料 — 例如安裝資料，安裝模組時必須添加這些資料。 這種機制允許在不同安裝之間轉移這些更改。

開發人員不會在重新安裝系統時重複執行手動SQL操作，而是建立包含資料的安裝（或升級）指令碼。 每次安裝模組時，指令碼都會運行。

## 這段錄像是給誰的？

- 開發人員

## 視頻內容

- 建立模組
- 建立InstallSchema指令碼
- 建立InstallData指令碼
- 添加新模組並驗證是否已建立包含資料的表
- 建立UpgradeSchema指令碼
- 建立UpgradeData指令碼
- 運行升級指令碼並驗證表是否已更改

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 有用資源

- [將安裝/升級指令碼遷移到聲明性架構](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
