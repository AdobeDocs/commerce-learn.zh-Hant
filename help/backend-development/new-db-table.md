---
title: 向資料庫添加表
description: '"[!DNL Commerce] 有一種特殊機制，可讓您建立資料庫表、修改現有表，甚至將一些資料添加到其中。」'
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
>不再建議使用，請參閱https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] 有一種特殊機制，可讓您建立資料庫表、修改現有表，甚至將一些資料添加到它們中 — 如安裝資料，安裝模組時必須添加這些資料。 此機制可讓這些變更在不同安裝之間轉讓。

在重新安裝系統時，開發人員不再重複執行手動SQL操作，而是建立包含資料的安裝（或升級）指令碼。 每次安裝模組時，指令碼都會執行。

## 這段錄像是給誰的？

- 開發人員

## 視訊內容

- 建立模組
- 建立InstallSchema指令碼
- 建立InstallData指令碼
- 新增模組並確認已建立含有資料的表格
- 建立UpgradeSchema指令碼
- 建立UpgradeData指令碼
- 運行升級指令碼並驗證表已更改

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 有用資源

- [將安裝/升級指令碼遷移到聲明性架構](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
