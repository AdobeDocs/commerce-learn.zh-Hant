---
title: 存貨狀態檢查：開發與績效
description: 瞭解如何評估是否需要在Adobe Commerce中進行即時詳細目錄檢查，並檢閱您商店的開發和效能考量事項。
feature: Best Practices, Inventory
topic: Development, Performance
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 496
last-substantial-update: 2024-05-09
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
    internal-label: Order Management System
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
    internal-label: 2FA
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
    internal-label: B2B
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: fcf0f5116ba75bd618f4ed80591ecc96132cdb45
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%
---
# 存貨狀態會檢查開發及效能的考量事項

庫存的準確性是一個重要的考量。 有些原生功能可協助確保此風險儘可能低，例如延期交貨訂單和設定缺貨臨界值。 這兩個主題都可以在[Adobe Experience League](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/inventory/configuration/backorders)上閱讀，以取得進一步說明。

有些專案和使用案例會要求Adobe Commerce商店進行即時詳細目錄狀態檢查。 本教學課程提供insight以處理此對話時與開發和效能考量事項。

## 驗證是否需要此請求

準備儘可能多地討論此要求。 最重要的是確認此專案不接受原生功能。 尋找此要求背後的原因，以驗證Adobe Commerce的原生功能無法滿足此要求。

另一個考量因素是開發、測試及維護此功能的成本。 利害關係人的意見不一定會成為必要條件。 在Adobe Commerce核心功能之外進行存貨驗證會產生相關成本。 這些成本來自技術債、更多測試和驗證，以及其架構的使用檔案和支援檔案。

## 決定可接受的存貨更新步調

嘗試考慮清查檢查以及它在3種方法中的完成方式。 每一種都有好處和限制。 這些錯誤也會增加複雜性，且需要針對錯誤處理進行更多測試和思考。 請記住，當您決定實作自訂解決方案時，會新增職責和考量事項。 範例包括後援程式、監控、測試和疑難排解，這些都屬於開發團隊。 需要包括的一些實用專案包括新的支援檔案、培訓和監控，以確保開發團隊可以支援整個功能。 副作用是開發團隊擁有該流程，且不再運用核心Adobe Commerce應用程式提供的原生功能。 Adobe支援無法協助進行此等級的自訂。

第一種方法是使用原生功能。 使用原生功能是風險最小的一項，且有許多優點。 採取此方法表示您可以仰賴Adobe Commerce所提供的所有現有檔案和教學課程來使用功能。 存貨管理有許多方面，因此使用應用程式隨附的內容為首要考量。 但在使用案例中，訂單時商務中找到的資料並不準確。 資料如何不同步的範例，是允許直接在Order Management系統中的Adobe Commerce應用程式外部進行銷售。 原因在於，若要確保在Adobe Commerce中呈現準確的詳細目錄層級，需要某種整合功能，才能讓Adobe Commerce資訊儘可能接近精確值。 如果超量銷售不被接受，則新增無存貨臨界值是在零之前停止銷售料號的好方法。 Adobe Commerce的原生同步功能每天最多1次。 此頻率在某些使用案例中已經足夠，但在其他案例中則不夠頻繁。 如需詳細資訊，請閱讀[排定的匯入和匯出](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)。

第二個方法是`near real-time`。 近乎即時仍使用原生功能。 不過，這包括提供整合的一些額外工作，這些整合會經常提供商務內容，以依排程更新其詳細目錄。 例如，每小時。 此選項需要考慮整合的運作方式，但使用「大量api」並擁有一些中介軟體來執行資料轉換並將其推送到商業是一種好方法。 檢視使用Adobe App Builder或類似平台執行大量工作，並以更頻繁的步調推送資訊至Adobe Commerce。

第三種方法最複雜，也最能承擔大部分的風險和責任，是對外部API或資料來源的即時庫存檢查。 對外部系統進行即時清查存在風險，並且有幾個其他元素需要考慮。 以下是需要評估的一小部分其他專案：

* 外部系統能否接受REST或GraphQL要求
* 端點是否有任何限制，例如每分鐘的X個請求數與網站流量不一致
* 載入時的回應時間有何變化
* 如果回應時間很長，會發生什麼情況？您會自動終止此專案，並使用備援選項，例如原生詳細目錄。
* 哪一種監控型別可用，以確保API請求在容許度限制內

## 非原生庫存管理的考量事項

儘可能保持自訂內容不複雜。
存貨的組織有多平坦，是1 sku與可用存貨的總量，還是需要考慮其他屬性。

如果存貨資訊相當穩定（例如sku和總可用數量），則會展開近乎即時的選項。 「近乎即時」的概念表示背景作業會從來源收集詳細目錄，然後填入儲存引擎，以用於回應請求。 為此，您可以使用Redis、Mongo或其他非關聯式資料庫。 這些選項非常快速，適用於索引鍵/值組。 如果資料較為複雜，則需要在商務應用程式內部或外部使用關係資料庫。 透過從商務資料庫解除安裝此專案，您可以將核心商務應用程式與這些交易隔離。 另一組優點是，可讓商務應用程式、CPU、RAM和其他應用程式的I/O停止使用。 若要從Adobe Commerce應用程式伺服器儲存資源，請善用新的API，從站外儲存空間提取資料。 此程式需要中介來協助轉換任何資料。 然後確定呼叫的應用程式可以如預期取得結果。 藉由使用Adobe App Builder和API網格，資料可以轉換並傳回格式正確的資料。

如果有多個詳細目錄來源，搭配API網狀使用Adobe App Builder也是很好的選擇。


## 將執行邏輯移出程式

Adobe Developer App Builder提供統一的第三方擴充性架構，用於整合及建立自訂體驗，以擴充Adobe解決方案。 Adobe Commerce可以使用Adobe Developer App Builder。 此方法非常適合用於擴充核心應用程式中通常會發生的某些功能，並將其移離現場。 從Commerce應用程式中移除功能，可減少Commerce應用程式的模組數量和複雜性。 數量較少的程式內自訂又可降低升級和維護的複雜性。

Adobe的團隊已建立一些檔案，作為靈感的絕佳來源，並提供工作程式碼範例，以啟發完成這項工作的方式。 當購物者新增產品至購物車時，協力廠商庫存管理系統會檢查該專案是否有庫存。 如果是，則允許新增產品。 否則，顯示錯誤訊息。 如需程式碼範例和進一步資訊，請前往[Webhook使用案例](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart)。

## 何時進行詳細目錄檢查

何時檢查庫存是否仍然可用取決於業務利害關係人，軟體架構師會提供其他關鍵利害關係人的一些輸入。 將專案新增至購物車及進入結帳工作流程時，需經過幾個適當的時間。 任何其他事件都會在不需要時將載入新增至後端系統。 請記住，目標是只有在清查問題最為重要時才能發現問題。 請謹慎考慮其他會影響詳細目錄狀態檢查整體目標的檢查，只有在利害關係人知道額外負載的潛在風險時，才允許進行這些檢查。

## 研究您的詳細目錄來源

需要全面調查外部存貨來源。 應該評估的專案包括API選項、GraphQL支援和預期的回應時間。 如果詳細目錄來源的連線頻寬有限或從未打算用於即時請求，則排除使用的能力，架構師需要考慮改為近乎即時。 如果API要求次數超過定義的引數，系統不會將此視為可行的選項。 此行為的範例是，一次性請求的API回應為200毫秒，但在中等負載下會升至500至900毫秒。 負載較多且會排除可用的即時詳細目錄呼叫，導致此情況變得更糟。

請務必使用簡單請求以及與已上線網站預期流量類似的高流量來測試API回應時間。 記得同時測試商務中的所有區域以模擬真實世界的情境。 如果在產品頁面、購物車中以及在結帳期間發生即時詳細目錄呼叫，則載入測試必須同時模擬所有這些，以模擬真實的客戶行為。

## 遞補選項

如果清查來源關閉且監控可用，建議使用Adobe Commerce的原生功能。 不過，只要適當地監控，客戶體驗可以動態變更，以反映即時存貨檢查的遺失。 這表示為了避免過度銷售，銷售或活動會提早取消或移除顯示畫面。 與店舖負責人討論遞補計畫，讓每個人都瞭解當存貨來源停止運作時會接管系統的自動流程。

## 結論

進行即時詳細目錄檢查的決定很重要。 確保網站所有者、開發團隊和其他人受過完整的教育，並瞭解所有好處和潛在陷阱取決於開發人員主管或架構師。 提供周到的計畫，其中涵蓋各種原因和遞補程式，是成功的關鍵。

即時詳細目錄檢查可以完成，但在QA週期期間需要圍繞測試和驗證進行研究和思考。 確保負載測試和端對端自動化測試可協助確保攔截並分類所有潛在問題。

如果監控偵測到失敗的呼叫或回應時間緩慢，請執行動作讓網站保持連線，並將客戶的不滿降至最低。 後援選項包含從使用原生功能到停用促銷活動、通知開發團隊或重新路由傳送請求至次要後端系統的選項。 因每個系統在某個時間點都會發生問題，所以應像實際整合一樣，謹慎規劃後援機制的實作方式。 任何自動化或需要手動操作的作業都應清楚記錄。
