---
title: 存貨狀態會檢查開發及效能的考量事項
description: 瞭解為Adobe Commerce執行詳細目錄狀態檢查的一些想法和考量事項。
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 498
last-substantial-update: 2024-05-09T00:00:00.000Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1982
ht-degree: 0%

---

# 存貨狀態會檢查開發及效能的考量事項

庫存的準確性是一個非常重要的考量。 有些原生功能可協助確保此風險儘可能低，例如延期交貨訂單和設定缺貨臨界值。 這兩個主題都可以在[Experience League](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/inventory/configuration/backorders)上閱讀，以取得進一步說明。

有些專案和使用案例會要求Adobe Commerce商店進行即時詳細目錄狀態檢查。 本教學課程提供insight以處理此對話時與開發和效能考量事項。

## 驗證此請求是否絕對必要

準備與儘可能多的資訊進行交談。 最重要的是確認此專案不接受原生功能。 尋找此要求背後的原因，以驗證Adobe Commerce的原生功能無法滿足此要求。

另一個考量因素是開發、測試及維護此功能的成本。 只是因為某些利害關係人認為有必要，並不意味著就是必要。 在Adobe Commerce核心功能之外進行存貨驗證會產生相關成本。 這些成本來自技術債、更多測試和驗證，以及其架構的使用檔案和支援檔案。

## 決定可接受的存貨更新步調

嘗試考慮清查檢查以及它在3種方法中的完成方式。 每一種都有好處和限制。 這些錯誤也會增加複雜性，且需要針對錯誤處理進行更多測試和思考。 請記住，當您決定採用自訂路由時，會新增職責和考量事項。 某些範例包括後援程式、監控、測試和疑難排解都屬於開發團隊。 需要包括的一些實用專案包括新的支援檔案、培訓和監控，以確保開發團隊可以支援整個功能。 另一個副作用，是開發團隊完全擁有該流程，並且不再運用核心Adobe Commerce應用程式提供的原生功能。 Adobe支援無法協助進行此等級的自訂。

第一種方法是使用原生功能。 使用原生功能是風險最小的一項，且有許多優點。 採取此方法表示您可以仰賴Adobe Commerce所提供的所有現有檔案和教學課程來使用功能。 存貨管理有許多方面，因此使用應用程式隨附的內容應該是首要考量。 但在使用案例中，訂單時商務中找到的資料可能不完全準確。 有些專案可能會不同步，其中一個範例是直接在訂單管理系統中，允許在Adobe Commerce應用程式外部進行銷售。 原因在於，若要確保在Adobe Commerce中呈現準確的詳細目錄層級，需要某種整合方式，才能讓Adobe Commerce資訊儘可能接近準確。 如果超量銷售不被接受，則新增無存貨臨界值是在零之前停止銷售料號的好方法。 Adobe Commerce的原生同步功能每天最多1次。 這對於某些使用案例來說已經足夠，但是對於其他使用案例來說可能不夠頻繁。 如需詳細資訊，請閱讀[排定的匯入和匯出](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)。

第二個方法是`near real-time`。 近乎即時仍使用原生功能。 不過，這包括提供整合的一些額外工作，這些整合會經常提供商務內容，以依排程更新其詳細目錄。 例如，每小時。 此選項需要一些整合如何運作的思考，但使用「大量api」和讓一些中介軟體執行資料轉換並將其推送到商業是一個好方法。 檢視使用Adobe App Builder或類似平台執行大量工作，並以更頻繁的步調推送資訊至Adobe Commerce。

第三種方法最複雜，也最能承擔大部分的風險和責任，是對外部API或資料來源的即時庫存檢查。 對外部系統進行即時清查存在風險，並且有幾個其他元素需要考慮。 以下是需要評估的一小部分其他專案：

* 外部系統能否接受REST或GraphQL要求
* 端點是否有任何限制，例如每分鐘的X個請求數，可能與網站流量不一致
* What happens to the response time under load
* What happens when the response times are long, do you terminate this automatically and use a fallback option such as the native inventory.
* What type of monitoring is available to ensure that API requests are within the tolerance limits

## Considerations when considering non-native inventory management

Keep the customizations as non-complex as possible.
How flat can the organization of the inventory be, is it just 1 sku and the total amount of available stock OR are there other attributes that need to be considered.

If the inventory information is fairly flat, for example a sku and the total available quantity, the options for near-real-time are expanded. The concept of near real-time means that there is a background operation that gathers the inventory from the source, and then populates a storage engine to be used to respond to the request. For this you can use things such as Redis, Mongo, or other non-relational databases. These options are nice because they are very fast and work great for key/value pairs. If the data is a bit more complex, then using a relation database, either inside or outside the commerce application, is likely to be required. By offloading this from the commerce database, you keep the core commerce application isolated from these transactions. Another set of benefits are saving the I/O from the commerce application, CPU, RAM and others from use. Instead of using the resources from the Adobe Commerce application servers take advantage of the new APIs to pull the data from the off site storage.  This will likely need a middleware to help transform any data. Then ensure that the calling application can get the result as expected. By using Adobe App Builder with API mesh the data can be transformed and returned properly formatted.

Using Adobe App Builder with API mesh is also a great option when there are multiple sources of inventory.


## Moving the execution logic to an out-of-process location

Adobe Developer App Builder provides a unified third-party extensibility framework for integrating and creating custom experiences to extend Adobe solutions. Adobe Commerce can use Adobe Developer App Builder. This would be an excellent use case for extending some functionality that normally occurs in the core application and moves it off-site. By removing functionality from the Commerce application this reduces the number of modules and complexity to the Commerce application. In turn, lower numbers of in-process customizations reduce the complexity for upgrading and maintenance.

For inspiration for how this might be accomplished, the team at Adobe has created some documentation that can be a great source of inspiration and provide working code samples. When a shopper adds a product to the cart, a third-party inventory management system checks whether the item is in stock. If it is, allow the product to be added. Otherwise, display an error message.  For code samples and further information go to [Webhook Use Cases](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## When to do inventory checks

When to check if inventory is still available will eventually be up to the business stake holder, the software architect with some input from other key stakeholders. A few appropriate times would include when adding an item to the cart and when entering the checkout workflow. Any other events will simply add load to the backend systems when it may not be necessary. Keep in mind that the goal is to catch an inventory issue only when it is paramount. Other checks may be nice to have but if they may impact the overall goal for inventory status checks, they should be carefully considered and only allowed if the business stakeholders or others are aware of the potential risk for extra load on the backend systems.

## Research how the inventory source

Comprehensive investigation of the external inventory source is required. Items that should be evaluated are available API options, support for GraphQL, and expected response times. If the inventory source has a limited connection bandwidth or was never intended to be used in a real time request, the ability for use is excluded and the architect needs to consider near-real-time instead.  If the API request times exceed the defined parameters it will also exclude this from being a viable option.  An example of this would be api responses are 200MS® for one a time requests, but rise to 500 to 900MS® under moderate load.  This would likely just get worse with more load and rules out live inventory calls from being available.

Be sure to test the api response times with simple requests as well as with a high volume similar to the expected traffic on the live website. Remember to test all areas from commerce at the same time to simulate real world scenarios. If the expectation is a live inventory call should occur on product pages, when viewing and editing a cart and during checkout, the load testing needs to simulate all of these at the same time to mimic real customer behavior and the expectation for a store.

## Fallback options

IF the inventory source is down and monitoring is available, using the native capability of Adobe Commerce is recommended. However, with proper monitoring the customer experience can dynamically change to reflect the loss of real time inventory checks. This may mean that a sale or event is canceled early or removed from display to avoid overselling. The expectation for what to do when the inventory source is down for any reason needs to be considered and discussed with the store owner so everyone is aware of any automatic process that takes over in that unfortunate circumstance.

## 結論

進行即時詳細目錄檢查的決定很重要。 確保網站所有者、開發團隊和其他人受過完整的教育，並瞭解所有好處和潛在陷阱取決於開發人員主管或架構師。 提供周到的計畫，其中涵蓋各種原因和遞補程式，是成功的關鍵。

即時詳細目錄檢查可以完成，但在QA週期期間需要圍繞測試和驗證進行研究和思考。 確保負載測試和端對端自動化測試可協助確保攔截並分類所有潛在問題。

如果監控偵測到外部系統呼叫失敗的趨勢，或如果回應時間超過可接受的臨界值，則考慮要執行哪些動作，以允許網站保持連線，並讓客戶獲得最少的惱怒。 後援選項可以簡單到關閉外部詳細目錄檢查並使用原生功能，也可以進階到停用促銷活動、通知開發團隊有升級問題，甚至可能將請求重新路由到第二個或第三個後端系統（如果可用）。 由於每項整合在某個時間點都會發生問題，因此如何運用遞補機制應僅作為實際實施來考慮。 任何自動化或需要手動操作的作業都應清楚記錄。
