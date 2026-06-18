---
title: Commerce入門套件中的最後一哩整合
description: 瞭解在Commerce中使用擴充性鉤子進行驗證、轉換、預先處理、傳送和後處理的最後一哩整合。
doc-type: Technical Video
duration: 557
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15869
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
TQID: https://experienceleague.adobe.com/TCR23A98L8XrVDEQeqLQoOXKQPBQu-Wb7YnGUkBXgak
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 342
ht-degree: 0%

---

# 使用Adobe Starter Kit進行最後一哩整合

瞭解開始與Adobe Commerce進行最後一哩整合時需考慮的專案，並專注於擴充性鉤點，以增強與協力廠商系統的連線能力。 此影片概述結構化方法，其中各種鉤點（例如驗證、轉換、前置處理、傳送和後處理）可確保順暢的資料流程和系統同步。 每個勾點都有不同的用途，包括：

* 根據結構描述驗證傳入資料
* 在系統之間轉換資料物件
* 在傳送相關資訊之前執行計算
* 正在傳送資料至目的地系統

請務必為每個區塊維護個別的JavaScript檔案，以維護商業邏輯完整性，並協助未來架構升級，確保健全且可調整的整合設定。

透過後處理勾點瞭解後處理活動的重要性，該勾點可讓使用者在資料同步後執行其他動作，例如新增評論到訂單或儲存外部ID。 影片包括最佳實務，例如將API請求封裝在特定程式庫中，以簡化與協力廠商系統的連線。 您還將瞭解每個鉤點的典型使用案例和處理不同情況的指南。

## 客群

* 想要瞭解擴充性鉤點的結構和功能，以及這些鉤點如何增強與協力廠商系統的連線性的開發人員。
* 開發人員想要瞭解與每個擴充功能掛接相關的典型使用案例和最佳實務（例如驗證、轉換、前置處理、傳送和後處理），以促進順暢的資料流程、系統同步化及有效的整合設定維護。&#x200B;URL

## 視訊內容

* 瞭解最後一哩整合中所叫用動作的結構。
* 瞭解驗證掛接內的典型使用案例，包括根據結構描述驗證傳入資料，以及根據特定條件略過特定事件。&#x200B;URL
* 了解轉換掛接在原始和目的地系統之間轉換資料物件時的角色。
* 瞭解傳送勾點對於促進實際資料傳送至目的地系統的重要性。

>[!VIDEO](https://video.tv.adobe.com/v/3451940?captions=chi_hant&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
