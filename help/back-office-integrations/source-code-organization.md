---
title: 瞭解關於Commerce Integration Starter Kit的關鍵資料夾和自動化指令碼說明
description: 瞭解Commerce整合入門套件中原始程式碼的組織。​URL
landing-page-description: 在Source Integration Starter Kit中探索Commerce程式碼組織
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: f0c6e9262a2bf2de3144255de1fc78d6972b6d33
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 適用於Adobe入門套件的Source程式碼組織

瞭解Adobe Commerce整合入門套件中的原始程式碼組織&#x200B;。 探索專案結構，醒目提示`actions`和`scripts`等關鍵資料夾及其各自的內容&#x200B;。 &#39;actions&#39;資料夾包含`ingestion`和`webhook`等子資料夾，其中包含事件處理和追蹤的必要程式碼。 您也會瞭解`starter-kit-info`和`scripts`資料夾。 `scripts`資料夾專注於自動化指令碼，例如`commerce-event-subscribe`和`onboarding`，可簡化專案中的事件設定和提供者設定。
&#x200B;URL
探索原始程式碼結構背後的邏輯，詳述每個實體資料夾下的`commerce`和`external`資料夾如何處理來自不同系統的事件。 影片說明`consumer`資料夾在將事件分派給適當的事件處理常式執行階段動作時的角色，以確保順暢處理。 影片也會說明在執行階段動作中實作的重試機制，以便有效處理失敗的事件。&#x200B;URL瞭解Adobe Commerce整合入門套件中原始程式碼的組織和功能，針對事件處理、自動化指令碼和設定提供有價值的深入分析。

## 對象

* 想要瞭解原始程式碼如何整理到重要資料夾（例如`actions`和`scripts`）的開發人員。
* 瞭解`actions`資料夾包含的子資料夾，例如`ingestion`和` webhook`，其中包含處理事件和追蹤部署的基本程式碼。
* 想要瞭解`actions`資料夾的開發人員，該資料夾包括如`customer`、`order`、`product`和`stock`等實體的資料夾。

## 視訊內容

* 瞭解四個主要資料夾： `actions`、`scripts`、`test`和`utils`，在工作階段期間著重於`actions`和`scripts`資料夾。&#x200B;URL
* 瞭解`actions`資料夾以及它如何包含重要的子資料夾，例如`ingestion`和`webhook`。
* 探索`actions`資料夾以及為何實體有特定資料夾，例如`customer`、`order`、`product`和`stock`，每個資料夾都包含結構化為`commerce`和`external`資料夾的執行階段動作，以便有效地管理Commerce和協力廠商系統的事件。&#x200B;URL
* 瞭解不要變更`starter-kit-info`資料夾中的程式碼的重要性，此資料夾包含Adobe用來根據入門套件追蹤專案部署的執行階段動作。&#x200B;URL
* 瞭解包含自動化指令碼（例如`commerce-event-subscribe`和`onboarding`）的`scripts`資料夾，這些指令碼可自動化Commerce中的事件設定、提供者設定和Adobe I/O事件模組的設定。&#x200B;URL

  >[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
