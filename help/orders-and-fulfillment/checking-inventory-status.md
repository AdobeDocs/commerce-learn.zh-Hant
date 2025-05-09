---
title: 存貨狀態會檢查開發及效能的考量事項
description: 瞭解為Adobe Commerce執行詳細目錄狀態檢查的一些想法和考量事項。
feature: Best Practices, Inventory
topic: Development, Performance
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: 84f9139181bf57d967494da97d4d27d297513c31
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---

# 存貨狀態會檢查開發及效能的考量事項

庫存的準確性是一個非常重要的考量。 有些原生功能可協助確保此風險儘可能低，例如延期交貨訂單和設定缺貨臨界值。 這兩個主題都可以在[Experience League](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/inventory/configuration/backorders)上閱讀，以取得進一步說明。

有些專案和使用案例會要求Adobe Commerce商店進行即時詳細目錄狀態檢查。 本教學課程深入分析如何透過開發和效能考量來處理此交談。

## 驗證此請求是否絕對必要

準備與儘可能多的資訊進行交談。 最重要的是確認此專案不接受原生功能。 尋找此要求背後的原因，以驗證Adobe Commerce的原生功能無法滿足此要求。

另一個考量因素是開發、測試及維護此功能的成本。 只是因為某些利害關係人認為有必要，並不意味著就是必要。 在Adobe Commerce核心功能之外進行存貨驗證會產生相關成本。 這些成本來自技術債、更多測試和驗證，以及其架構的使用檔案和支援檔案。

## 決定可接受的存貨更新步調

嘗試考慮清查檢查以及它在3種方法中的完成方式。 每一種都有好處和限制。 這些錯誤也會增加複雜性，且需要針對錯誤處理進行更多測試和思考。 請記住，當您決定採用自訂路由時，會新增職責和考量事項。 某些範例包括後援程式、監控、測試和疑難排解都屬於開發團隊。 需要包括的一些實用專案包括新的支援檔案、培訓和監控，以確保開發團隊可以支援整個功能。 另一個副作用，是開發團隊完全擁有該流程，並且不再運用核心Adobe Commerce應用程式提供的原生功能。 Adobe支援無法協助進行此自訂層級。

第一種方法是使用原生功能。 使用原生功能是風險最小的一項，且有許多優點。 採取此方法表示您可以仰賴Adobe Commerce所提供的所有現有檔案和教學課程來使用功能。 存貨管理有許多方面，因此使用應用程式隨附的內容應該是首要考量。 但在使用案例中，訂單時商務中找到的資料可能不完全準確。 有些專案可能會不同步，其中一個範例是直接在訂單管理系統中，允許在Adobe Commerce應用程式外部進行銷售。 原因在於，若要確保在Adobe Commerce中呈現準確的詳細目錄層級，需要某種整合方式，才能讓Adobe Commerce資訊儘可能接近準確。 如果超量銷售不被接受，則新增無存貨臨界值是在零之前停止銷售料號的好方法。 Adobe Commerce的原生同步功能每天最多1次。 這對於某些使用案例來說已經足夠，但是對於其他使用案例來說可能不夠頻繁。 如需詳細資訊，請閱讀[排定的匯入和匯出](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)。

第二個方法是`near real-time`。 近乎即時仍使用原生功能。 不過，這包括提供整合的一些額外工作，這些整合會經常提供商務內容，以依排程更新其詳細目錄。 例如，每小時。 此選項需要一些整合如何運作的思考，但使用「大量api」和讓一些中介軟體執行資料轉換並將其推送到商業是一個好方法。 檢視使用AdobeApp Builder或類似平台完成大量工作，並以更頻繁的步調推送資訊至Adobe Commerce。

第三種方法最複雜，也最能承擔大部分的風險和責任，是對外部API或資料來源的即時庫存檢查。 對外部系統進行即時清查存在風險，並且有幾個其他元素需要考慮。 以下是需要評估的一小部分其他專案：

* 外部系統能否接受REST或GraphQL要求
* 端點是否有任何限制，例如每分鐘的X個請求數，可能與網站流量不一致
* 載入時的回應時間有何變化
* 如果回應時間很長，會發生什麼情況？您會自動終止此專案，並使用備援選項，例如原生詳細目錄。
* 哪一種監控型別可用，以確保API請求在容許度限制內

## 考慮非原生庫存管理時的注意事項

儘可能保持自訂內容不複雜。
存貨組織是否平坦，是否只有1個SKU和可用存貨的總量，或者還有其他屬性需要考慮。

如果存貨資訊相當穩定（例如sku和總可用數量），則會展開近乎即時的選項。 「近乎即時」的概念表示背景作業會從來源收集詳細目錄，然後填入儲存引擎，以用於回應請求。 為此，您可以使用Redis、Mongo或其他非關聯式資料庫。 這些選項很好，因為它們非常快速，非常適用於索引鍵/值配對。 如果資料較為複雜，則可能需要在商務應用程式內部或外部使用關係資料庫。 透過從商務資料庫解除安裝此專案，您可以將核心商務應用程式與這些交易隔離。 另一組優點是將商務應用程式的I/O、CPU、RAM和其他元件儲存在使用中。 您不必透過Adobe Commerce應用程式伺服器使用資源，而是運用新的API從站外儲存空間提取資料。  這可能需要中介軟體來協助轉換任何資料。 然後確定呼叫的應用程式可以如預期取得結果。 透過AdobeApp Builder和API網格，資料可以轉換並傳回格式正確的資料。

如果有多個詳細目錄來源，搭配API網狀使用AdobeApp Builder也是很好的選擇。


## 將執行邏輯移至程式外位置

Adobe Developer App Builder提供統一的第三方擴充性架構，用於整合及建立自訂體驗，以擴充Adobe解決方案。 Adobe Commerce可以使用Adobe Developer App Builder。 這將是一個絕佳的使用案例，可用來擴充核心應用程式中通常會發生的部分功能，並將其移離現場。 從Commerce應用程式中移除功能，可減少Commerce應用程式的模組數量和複雜性。 數量較少的程式內自訂又可降低升級和維護的複雜性。

為了啟發如何達成此目標，Adobe的團隊已建立一些檔案，這些檔案可以成為靈感的絕佳來源，並提供工作程式碼範例。 當購物者新增產品至購物車時，協力廠商庫存管理系統會檢查該專案是否有庫存。 如果是，則允許新增產品。 否則，顯示錯誤訊息。  如需程式碼範例和進一步資訊，請前往[Webhook使用案例](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart)。

## 何時進行詳細目錄檢查

何時檢查存貨是否仍然可用，最終將取決於業務利害關係人、軟體架構師，以及來自其他主要利害關係人的一些輸入。 將專案新增至購物車及進入結帳工作流程時，可能會有數個適當的時間。 任何其他事件都只會在不需要時將載入新增到後端系統。 請記住，目標是只有在清查問題最為重要時才能發現問題。 可能有其他檢查可能很好，但如果這些檢查會影響詳細目錄狀態檢查的整體目標，則只有在業務利害關係人或其他人知道後端系統可能會有額外負載的風險時，才應謹慎考慮並允許。

## 研究存貨來源如何

需要全面調查外部存貨來源。 應該評估的專案包括API選項、GraphQL支援和預期的回應時間。 如果詳細目錄來源的連線頻寬有限或從未打算用於即時請求，則排除使用的能力，架構師需要考慮改為近乎即時。  如果API要求次數超過定義的引數，它也會將此排除在可行選項之外。  例如，API回應是針對一次請求的200MS®，但在中等負載下會升至500至900MS®。  隨著更多負載並排除可用的即時詳細目錄呼叫，情況可能會變得更糟。

請務必使用簡單請求以及與已上線網站預期流量類似的高流量來測試API回應時間。 記得同時測試商務中的所有區域以模擬真實世界的情境。 如果期望是即時詳細目錄呼叫應該發生在產品頁面上，當檢視和編輯購物車以及在結帳期間，載入測試需要同時模擬所有這些內容，以模擬真實的客戶行為和商店的期望。

## 遞補選項

如果清查來源關閉且監控可用，建議使用Adobe Commerce的原生功能。 不過，只要適當地監控，客戶體驗可以動態變更，以反映即時存貨檢查的遺失。 這可能表示為了避免過度銷售，銷售或活動會提早取消或移除顯示畫面。 當存貨來源因任何原因停止運作時，預期要採取的行動需要與店舖負責人進行考量及討論，讓每個人都能知道在這種不幸的情況下會接管任何自動程式。

## 結論

進行即時詳細目錄檢查的決定很重要。 確保網站所有者、開發團隊和其他人受過完整的教育，並瞭解所有好處和潛在陷阱取決於開發人員主管或架構師。 提供周到的計畫，其中涵蓋各種原因和遞補程式，是成功的關鍵。

即時詳細目錄檢查可以完成，但在QA週期期間需要圍繞測試和驗證進行研究和思考。 確保負載測試和端對端自動化測試可協助確保攔截並分類所有潛在問題。

如果監控偵測到外部系統呼叫失敗的趨勢，或如果回應時間超過可接受的臨界值，則考慮要執行哪些動作，以允許網站保持連線，並讓客戶獲得最少的惱怒。 後援選項可以簡單到關閉外部詳細目錄檢查並使用原生功能，也可以進階到停用促銷活動、通知開發團隊有升級問題，甚至可能將請求重新路由到第二個或第三個後端系統（如果可用）。 由於每項整合在某個時間點都會發生問題，因此如何運用遞補機制應僅作為實際實施來考慮。 任何自動化或需要手動操作的作業都應清楚記錄。
