---
title: Commerce整合入門套件中的最後一哩整合。
description: Commerce中的「最後一公里」整合，強調可擴充性鉤點，例如驗證、轉換、預先處理、傳送和後處理​。
landing-page-description: 瞭解Commerce系統最後一哩整合中的擴充性鉤點的結構和功能。
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# 使用Adobe入門套件進行最後一哩整合

瞭解開始與Adobe Commerce進行最後一哩整合時要考慮的專案，重點放在使用擴充性鉤子以增強與協力廠商系統的連線。 此影片概述結構化方法，其中各種鉤點（例如驗證、轉換、前置處理、傳送和後處理）可確保順暢的資料流程和系統同步。 每個勾點都有不同的用途，包括：

* 根據結構描述驗證傳入資料
* 在系統之間轉換資料物件
* 在傳送相關資訊之前執行計算
* 正在傳送資料至目的地系統

請務必為每個區塊維護個別的JavaScript檔案，以維護商業邏輯完整性，並協助未來架構升級，確保健全且可調整的整合設定。

透過後處理勾點瞭解後處理活動的重要性，該勾點可讓使用者在資料同步後執行其他動作，例如新增評論到訂單或儲存外部ID。 影片包括最佳實務，例如將API請求封裝在特定程式庫中，以簡化與協力廠商系統的連線。 您還將瞭解每個鉤點的典型使用案例和處理不同情況的指南。

## 對象

* 想要瞭解擴充性鉤點的結構和功能，以及這些鉤點如何增強與協力廠商系統的連線性的開發人員。
* 開發人員想要瞭解與每個擴充功能掛接相關的典型使用案例和最佳實務（例如驗證、轉換、前置處理、傳送和後處理），以促進順暢的資料流程、系統同步化及有效的整合設定維護。&#x200B;URL

## 視訊內容

* 瞭解最後一哩整合中所叫用動作的結構。
* 瞭解驗證掛接內的典型使用案例，包括根據結構描述驗證傳入資料，以及根據特定條件略過特定事件。&#x200B;URL
* 了解轉換掛接在原始和目的地系統之間轉換資料物件時的角色。
* 瞭解傳送勾點對於促進實際資料傳送至目的地系統的重要性。

>[!VIDEO](https://video.tv.adobe.com/v/3451940?learn=on&captions=chi_hant)

{{$include /help/_includes/starter-kit-related-links.md}}
