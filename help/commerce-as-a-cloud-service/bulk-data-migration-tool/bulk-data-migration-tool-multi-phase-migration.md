---
title: 大量資料遷移工具 — 多階段遷移
description: 瞭解在生產轉換期間來源必須保持凍結時，如何使用維護模式搭配大量資料移轉工具執行多階段移轉。
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# 使用大量資料遷移工具執行多階段遷移

當您的來源環境在擷取期間必須凍結時，請執行多階段移轉，非常適合在生產轉換中，新訂單無法在移轉中到達。 它使用維護模式，並有五個必須依序執行的階段。 如果您的來源可以保持上線，請改為觀看此系列中的單階段移轉影片。

## 這部影片是給誰看的？

* 解決方案架構師
* DevOps工程師
* 後端開發人員

## 視訊內容

* 開始前的一項主要區別：針對移轉工具本身執行`bin/console`命令；在您的來源Commerce伺服器上執行`bin/magento maintenance`命令。 此工具不會為您啟用或停用維護模式 — 這是手動步驟。
* 第一階段會在來源仍在作用中時執行 — `bin console migration:before-maintenance`會檢查設定、初始化環境、連線至CDMS、註冊移轉、執行功能測試，以及建立綜合測試資料。 在此階段完成之前不要啟用維護模式。
* 第三階段是從凍結環境中擷取 — 如有需要，`bin/console migration:during-maintenance`會重新開啟PaaS通道、從來源擷取、清除中繼檢視、載入ACCS目標、執行驗證，以及清除目標上的測試資料。

>[!VIDEO](https://video.tv.adobe.com/v/3496422?captions=chi_hant&learn=on)
