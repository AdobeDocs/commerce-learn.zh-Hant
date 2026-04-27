---
title: 品質修補工具
description: Learn how to use the quality patch tool when diagnosing a problem, finding a solution and applying a patch found in the existing list of patches available.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 593
ht-degree: 0%

---

# Quality patch tool

Learn how to use the quality patch tool when diagnosing a problem, finding a solution and applying a patch found in the existing list of patches available.

## 您將瞭解的內容

Learn how to triage an issue, then use some basic techniques to find a quality patch to apply a fix.

## 客群

* Developers learning how to find issues and leverage this tool for applying GIT patches for known issues

## 視訊內容

The Quality Patches Tool is a command-line utility for Adobe Commerce and Magento Open Source. Here&#39;s what it allows you to do:

* View general information about the latest quality patches.
* Apply quality patches to your installation.
* Revert applied patches if needed

These patches are developed from Adobe Developers the Magento Open Source community to enhance stability and performance. Keep in mind that it&#39;s not recommended for applying large numbers of patches, as it can complicate future upgrades.

>[!VIDEO](https://video.tv.adobe.com/v/3454082?captions=chi_hant&learn=on)

## Why use the quality patch tool

You might want to use the Quality Patches Tool for Adobe Commerce or Magento Open Source if you&#39;re looking to:

Enhance stability and performance: Quality patches address issues, improve security, and optimize your installation.
Stay up-to-date: Applying patches ensures that your system is current and protected.
Revert changes: If a patch causes unexpected issues, you can revert it using the tool. Remember, it&#39;s best suited for applying a limited number of patches to avoid complicating future upgrades.  

## Limitations or concerns with using the quality patch tool

While the Quality Patches Tool offers benefits, there are a few considerations to keep in mind:

* Compatibility: Ensure that the patches are compatible with your specific version of Adobe Commerce or Magento Open Source.
* Testing: Always test patches in a staging environment before applying them to production. 可能會出現非預期的問題。
* 修正程式相依性：某些修正程式可能相依於其他修正程式。 請注意任何必要條件。
* 自訂：如果您進行了自訂程式碼變更，修補程式可能會發生衝突。 請仔細檢閱變更。
* 備份：在套用修補程式之前先備份您的安裝，以避免資料遺失。

雖然「品質修補程式工具」對於套用有限數目的修補程式很有用，但不建議用來處理大量修補程式。 套用太多修補程式會讓未來的升級和維護工作變得複雜。 如果您有許多修補程式可套用，請考慮其他方法或諮詢Magento專家。 

## 摘要

品質修補程式工具可讓電子商務平台套用修補程式，以提升穩定性和安全性。 這些修補程式可解決問題、改善效能，並最佳化系統。 讓您的安裝保持最新狀態，可確保防範漏洞。

在套用修補程式之前，必須在預備環境中測試修補程式。 確保與您特定的Adobe Commerce或Magento Open Source版本相容。 部分修補程式可能有相依性，因此請仔細檢視先決條件。

 請在套用修補程式之前備份您的安裝，以防止資料遺失。 如果您變更了自訂程式碼，請注意修補程式可能會發生衝突。 遵循最佳實務並監控每個修補程式的影響。

## 相關文章和影片

* [品質修補工具搜尋](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hant)
* [發行說明](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [修補程式的Github](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [使用品質修補工具](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/quality-patches-tool/usage)
* [QPT技術影片](https://experienceleague.adobe.com/zh-hant/docs/commerce-learn/tutorials/tools/quality-patch-tool)
