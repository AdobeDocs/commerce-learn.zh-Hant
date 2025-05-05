---
title: 使用重試機制的原生功能
description: 針對彈性應用程式運用Adobe I/O事件的重試機制，包括重試條件和視覺指示器。
landing-page-description: 瞭解並運用Adobe I/O事件的內建重試機制，以增強應用程式復原力並有效管理事件啟用。
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: eb548dd83e3bab7a4a1486cd2cbd88efcc060121
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# 利用Adobe I/O事件重試機制提高應用程式彈性

影片概述運用Adobe I/O事件內建重試機制的全面指南，以增強應用程式復原能力。 瞭解特定HTTP回應狀態程式碼如何觸發事件重試。 Adobe I/O事件採用指數和固定的重試後退策略，間隔從1分鐘增加到15分鐘。 本檔案也詳細說明重試指標如何出現在開發人員控制檯中，提供警告圖示等視覺提示和分別表示失敗和重試事件的圓形箭頭。

瞭解重試機制如何在「消費者」執行階段動作的內容中運作，並決定是否重試事件。 成功的回應會以200狀態代碼表示，而錯誤回應則包含具有「statusCode」屬性的錯誤物件。 「取用者」執行階段動作會根據下游處理結果決定要傳回的HTTP回應代碼，確保高效的事件處理和最終成功的啟用。

## 對象

* 開發人員瞭解會觸發事件重試的特定HTTP回應狀態代碼。
* 團隊如果想要瞭解Adobe I/O事件在重試時所使用的指數和固定後退策略。
* 開發人員想瞭解開發人員主控台中的視覺指標如何代表失敗和重試的事件。

## 視訊內容

* Adobe I/O事件具有內建的立即可用的重試機制，可根據特定HTTP回應狀態代碼自動重試事件啟用。
* Adobe I/O事件實作的重試機制涉及指數和固定的後退策略。
* 開發人員控制檯中的視覺指示器，例如失敗事件的警告圖示和重試事件的圓形箭頭圖示。
* 「取用者」執行階段動作在決定事件處理的適當HTTP回應狀態程式碼時，扮演了重要的角色。

>[!VIDEO](https://video.tv.adobe.com/v/3449084?learn=on&captions=chi_hant)

{{$include /help/_includes/starter-kit-related-links.md}}

## 相關檔案

* [Webhook無法處理事件](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [已關閉Webhook並標籤為不穩定](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
