---
title: 大量資料移轉工具 — 單階段移轉
description: 瞭解如何使用大量資料移轉工具執行單階段移轉，以進行試執行以及來源在擷取期間可以維持即時狀態的環境。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00.000Z
jira: KT-22139
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%
---
# 使用大量資料移轉工具執行單階段移轉

當您的來源環境在擷取期間可保持即時時，執行單階段移轉 — 適用於練習以及開發或沙箱環境。 如果您需要凍結的來源，例如生產轉換，新訂單無法在移轉期間進入，請改為觀看此系列中的分階段移轉影片。

## 這部影片是給誰看的？

* 解決方案架構師
* DevOps工程師
* 後端開發人員

## 視訊內容

* 使用`bin console build`建置Docker映像 — 只有在Dockerfile變更時才重新執行此映像。
* 若要啟動CDMS CLI容器管理員，請執行`bin console start`，然後開啟容器中的殼層一次，即可下載其相依性。
* 若要執行完整的十步驟管道，請執行`bin console migration`：檢查設定、初始化環境、開啟PaaS通道、執行整合測試、向CDMS註冊、分析目標結構描述、產生測試資料、擷取來源資料、載入ACCS、驗證總和檢查碼、清除和彙總。
* 檢查移轉摘要報告 — 步驟8 （資料完整性驗證）會記錄失敗而不停止管道，因此完成的執行不保證可乾淨驗證。
* 這個單階段命令是一個完整、獨立的管道 — 請勿在維護模式（分階段移轉）工作流程中將其用作步驟，因為工作流程有其自己的專用命令。

>[!VIDEO](https://video.tv.adobe.com/v/3496316?learn=on)
