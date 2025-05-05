---
title: 品質修補工具
description: 瞭解在診斷問題、尋找解決方案並套用現有可用修補程式清單中的修補程式時，如何使用品質修補工具。
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: e306b2cd26506f6a7ef37c2d416be7172dc3c0d2
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# 品質修補工具

瞭解在診斷問題、尋找解決方案並套用現有可用修補程式清單中的修補程式時，如何使用品質修補工具。

## 您將瞭解的內容

瞭解如何分類問題，然後使用一些基本技術來尋找品質修補程式以套用修正。

## 客群

* 開發人員瞭解如何尋找問題，並運用此工具套用GIT修補程式來解決已知問題

## 視訊內容

「品質修補工具」是適用於Adobe Commerce和Magento Open Source的命令列公用程式。 以下是它可讓您執行的動作：

* 檢視最新品質修補程式的一般資訊。
* 將高品質的修補程式套用至您的安裝。
* 視需要還原已套用的修補程式

這些修補程式是由Adobe開發人員Magento Open Source社群所開發，可增強穩定性和效能。 請記住，不建議套用大量修補程式，因為這會使未來的升級複雜化。

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## 為何使用品質修補工具

如果您想要執行下列作業，建議您使用Adobe Commerce或Magento Open Source的「品質修補工具」：

增強穩定性和效能：高品質的修補程式可解決問題、改善安全性，並最佳化您的安裝。
保持最新狀態：套用修補程式可確保您的系統為最新狀態並受到保護。
回覆變更：如果修補程式造成非預期的問題，您可以使用工具回覆。 請記住，它最適合套用數量有限的修補程式，以免讓未來的升級變得複雜。  

## 使用品質修補工具的限制或疑慮

雖然「品質修補程式」工具有其優點，但請謹記以下注意事項：

* 相容性：確保修補程式與您特定的Adobe Commerce或Magento Open Source版本相容。
* 測試：永遠在將修補程式套用至生產環境之前，先在中繼環境中測試修補程式。 可能會出現非預期的問題。
* 修正程式相依性：某些修正程式可能相依於其他修正程式。 請注意任何必要條件。
* 自訂：如果您進行了自訂程式碼變更，修補程式可能會發生衝突。 請仔細檢閱變更。
* 備份：在套用修補程式之前先備份您的安裝，以避免資料遺失。

雖然「品質修補程式工具」對於套用有限數目的修補程式很有用，但不建議用來處理大量修補程式。 套用太多修補程式會讓未來的升級和維護工作變得複雜。 如果您有許多修補程式可套用，請考慮其他方法或諮詢Magento專家。 

## 摘要

品質修補程式工具可讓電子商務平台套用修補程式，以提升穩定性和安全性。 這些修補程式可解決問題、改善效能，並最佳化系統。 讓您的安裝保持最新狀態，可確保防範漏洞。

在套用修補程式之前，必須在預備環境中測試修補程式。 確保與您特定版本的Adobe Commerce或Magento Open Source相容。 部分修補程式可能有相依性，因此請仔細檢視先決條件。

 請在套用修補程式之前備份您的安裝，以防止資料遺失。 如果您變更了自訂程式碼，請注意修補程式可能會發生衝突。 遵循最佳實務並監控每個修補程式的影響。

## 相關文章和影片

* [品質修補工具搜尋](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [發行說明](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [修補程式的Github](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [使用品質修補工具](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* QPT[&#128279;](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)上的技術影片
