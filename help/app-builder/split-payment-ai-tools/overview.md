---
title: 分割付款POC — App Builder和AI工具
description: 瞭解使用App Builder和Commerce PaaS的分割付款概念證明，包括目標、架構和此第一場會議涵蓋的內容。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 237
jira: KT-20791
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 0%

---

# 建立分割付款POC：App Builder和AI工具

這是第一組教學課程，向您介紹使用AI輔助開發，幫助您建立分割付款概念證明。 您在雲端(PaaS)或內部部署中使用Adobe App Builder和Adobe Commerce，從此工作階段的概觀移至後續的實作教學課程。 期待目標、高階架構，以及系列其他涵蓋範圍的藍圖。

## 影片

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## 重要詳細資訊


本教學課程跨越大部分Adobe Commerce團隊現今所在之處（深入探討PHP）與Adobe希望他們前往之處（在Adobe App Builder中的Commerce外部執行事件導向、無伺服器的商業邏輯）之間的差距。 它透過真實的工作功能而非玩具範例來完成這項工作。

### 您實際建置的內容

客戶使用「貨到付款」與「商店貸方」的組合付款的分割付款系統。 下訂單後，操作員（或ERP系統）會透過獨立控制面板確認或拒絕現金付款，而無須開啟Commerce Admin。 整個接受/拒絕工作流程會在App Builder中執行。

#### 建築課程（核心教學）

本教學課程示範審慎且可重複的決策架構：

* **在PHP中必須保留的內容：**&#x200B;在Commerce請求週期中同步執行的任何內容，或呼叫沒有乾淨外部表面的Commerce內部API的任何內容
* **哪些專案移至App Builder：**&#x200B;其他專案 — 事件處理、運運算元工作流程、外部整合，以及面向運運算元的工具

這並不是「將所有內容移至App Builder」。 對於需要在不重寫的情況下開始轉換的團隊來說，這是一個實用、誠實的起點。

### 為何不包含PHP程式碼

AI提示方法實際上比此使用案例的範常式式碼更好，因為提示會擷取行為合約和架構推理，而範常式式碼無法單獨傳達這些合約。 執行提示並讀取輸出的開發人員瞭解程式碼為何會依原樣塑造，而不只是看起來如何。

### 包含的內容

* 完成App Builder應用程式程式碼（跨專案一致 — 直接使用或作為參考）
* 為Cursor和Claude設計的一整套編號AI提示，包括Commerce模組、App Builder Orchestrator、操作員儀表板、測試和生產路徑
* 模擬指令碼，用於針對即時Commerce網站測試接受/拒絕流程，而不需要真正的ERP
* 說明每項決策的架構檔案

### 適用對象

Adobe Commerce內部部署或Commerce Cloud上的開發人員，正展開其首次真正的App Builder整合。 並非針對完全無頭式API優先部署，特別是針對轉型中、執行傳統店面的團隊，他們想要瞭解如何在不放棄現有PHP投資的情況下將真實商業邏輯解除安裝到App Builder。

### 必要條件圖說文字

Adobe Commerce 2.4.5或更新版本、存取Adobe Developer Console組織和App Builder專案，以及啟用I/O事件。 結帳UI假定為乾淨的Luma主題 — 自訂主題需要先調整提示才能執行。

### 最終想法

這應視為有關人工智慧輔助開發的入門級討論。 本教學課程是示範如何在Commerce開發工作流程中使用AI工具，而不僅僅是有關分割支付的教學課程。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
