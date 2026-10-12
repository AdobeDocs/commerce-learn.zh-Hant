---
title: Adobe Commerce上市後、上市後以及持續監控和疑難排解
description: 瞭解準備和啟動網站的最佳實務和秘訣。 瞭解如何選擇正確的測試環境、在支援票證中提供相關詳細資訊，以及使用與支援團隊相同的監控工具來促進溝通。
feature: Best Practices, Cloud
topic: Architecture, Commerce, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3576
last-substantial-update: 2024-05-07T00:00:00.000Z
jira: KT-15467
exl-id: e3748ec2-b449-4cc2-927d-e7ba4f4dbaf8
TQID: 'https://experienceleague.adobe.com/sSK6X9LrKA5LUgdELTzfyNZu8RP9juyYGDdGQfPni4w'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
role_v2:
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%
---
# Adobe Commerce Cloud的啟動、監控和疑難排解

此網路研討會提供準備和啟動網站的幾個最佳實務和提示。 它強調選擇適當的測試環境並在支援票證中提供準確詳細資訊的重要性，以促進與支援團隊的有效溝通。 建議您使用與支援團隊相同的監控工具（例如New Relic），以便更妥善地共同作業和解決問題。 重點說明對周邊系統（例如支付處理器）進行徹底測試，以確保其在啟動期間能夠正常運作。 本檔案也強調需要預測和規劃潛在的生產問題，例如效能瓶頸和快取預熱，以確保順利成功啟動。 瞭解共擔責任模式是討論的另一個重要方面，其中澄清了某些任務（如資料庫還原和應用程式程式碼安全性）是網站所有者的責任。 依照這些最佳實務，可以簡化網站啟動作業、主動解決潛在問題，並讓啟動成功進行

## 客群

* 開發團隊、經理、主要開發人員、技術架構師
* 團隊以升級、移轉或新商務產品的形式實作Adobe Commerce

## 視訊內容

* 選擇適當的測試環境並在票證中提供相關詳細資訊。
* 使用與支援團隊相同的監控工具(New Relic)，以獲得更好的溝通和問題解決。
* 描述問題時提供熒幕擷取畫面或New Relic連結，以協助支援人員更有效瞭解及疑難排解。
* 瞭解共擔責任模式，特別是資料庫還原和安全性方面。
* 測試周邊系統（例如支付處理器），並確保在轉換期間能正確處理訂單ID。
* 在生產期間預測和規劃潛在問題，例如效能瓶頸和快取預熱的需求。


>[!VIDEO](https://video.tv.adobe.com/v/3428990?learn=on)

{{$include /help/_includes/enablement-series-related-links.md}}
