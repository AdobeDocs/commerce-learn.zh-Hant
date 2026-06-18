---
title: 使用重試機制的原生功能
description: 瞭解如何使用Adobe I/O Events的重試機制來建立復原性應用程式，包括重試條件、後退策略和視覺指示器。
doc-type: Technical Video
duration: 402
last-substantial-update: 2024-07-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15872
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
TQID: https://experienceleague.adobe.com/hrzcmSY8cAke4LBLRtqfkP8-t6jP4KMoMc7iL3WPRng
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 382
ht-degree: 0%

---

# 使用Adobe I/O Events重試機制以增強應用程式彈性

影片概述運用Adobe I/O Events內建重試機制以加強應用程式復原能力的全面指南。 瞭解特定HTTP回應狀態程式碼如何觸發事件重試。 Adobe I/O Events採用指數和固定的重試後退策略，間隔從1分鐘增加到15分鐘。 本檔案也詳細說明重試指標如何出現在開發人員控制檯中，提供警告圖示等視覺提示和分別表示失敗和重試事件的圓形箭頭。

瞭解重試機制如何在「消費者」執行階段動作的內容中運作，並決定是否重試事件。 成功的回應會以200狀態代碼表示，而錯誤回應則包含具有「statusCode」屬性的錯誤物件。 「取用者」執行階段動作會根據下游處理結果決定要傳回的HTTP回應代碼，確保高效的事件處理和最終成功的啟用。

## 客群

* 開發人員瞭解會觸發事件重試的特定HTTP回應狀態代碼。
* 團隊如果想要瞭解Adobe I/O Events在重試時所使用的指數和固定後退策略。
* 開發人員想瞭解開發人員主控台中的視覺指標如何代表失敗和重試的事件。

## 視訊內容

* Adobe I/O Events有內建的立即可用重試機制，可根據特定HTTP回應狀態代碼自動重試事件啟用。
* Adobe I/O Events實作的重試機制涉及指數式和固定的退避策略。
* 開發人員控制檯中的視覺指示器，例如失敗事件的警告圖示和重試事件的圓形箭頭圖示。
* 「取用者」執行階段動作在決定事件處理的適當HTTP回應狀態程式碼時，扮演了重要的角色。

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 相關檔案

* [Webhook無法處理事件](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook已關閉並標籤為不穩定](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
