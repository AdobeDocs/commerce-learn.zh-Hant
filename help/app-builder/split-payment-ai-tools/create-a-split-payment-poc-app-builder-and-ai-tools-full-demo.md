---
title: 建立分割付款POC：App Builder完整示範
description: 瞭解分割付款、REST、App Builder I/O和操作員在此Luma示範中如何接受/拒絕運作，加上可設定封鎖購物車的預訂單總計。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 955
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: b3a9cee9ab59307883444650e8ee2423ab630b6b
workflow-type: tm+mt
source-wordcount: '1050'
ht-degree: 0%

---

# 建立分割付款POC：App Builder完整示範

這是以Adobe Commerce和Adobe App Builder為基礎的分割付款概念驗證的端對端逐步解說。 此示範會假設您已使用AI工具和提示來產生程式中Commerce擴充功能和App Builder應用程式；影片會說明合併程式碼、使用原生Luma主題部署至Adobe Commerce Cloud網站，且App Builder專案已上線後會發生什麼事。

購物者支付部分現金和部分&#x200B;**[!UICONTROL Store Credit]**。 Commerce擁有同步結帳以及店面需求的API；App Builder可處理協調流程、操作員工作流程和I/O事件消費者。 參考實作使用Commerce (PaaS)專案和Luma原生結帳，而非Edge Delivery Services店面，這是許多商家的常見方式。 如果您在其他拓撲中使用&#x200B;**Adobe Commerce as a Cloud Service**，App Builder程式碼會維持類似，但店面和程式中的工作會有所不同。 針對在Luma上雲端中的內部部署、自行託管和Commerce，本影片說明處理程式內程式碼與App Builder之間用於新功能的實用劃分。

## 影片

>[!VIDEO](https://video.tv.adobe.com/v/3484087?learn=on)

## 這部影片是給誰看的？

* 規劃Commerce擴充性的技術架構師
* 實施簽出和整合的開發人員
* 需要在PHP和App Builder之間實際分割的實作團隊

## 在店面設定情境

示範帳戶有&#x200B;**[!UICONTROL Store Credit]**，而您在結帳時看到該餘額。 新增產品，直到訂單總計約$80為止。 這個大小讓您有空間同時顯示成功的接受路徑和拒絕路徑，就像卡片拒絕一樣，而不用數學運算來抗爭。

**[!UICONTROL Split payment]**&#x200B;僅提供給已登入的客戶，因為工作階段必須公開商店點數。 來賓簽出不顯示分割選項。

1. 在&#x200B;**[!UICONTROL Payment]**&#x200B;步驟上，選擇現金方式（示範會將&#x200B;**[!UICONTROL Cash on delivery]**&#x200B;重新命名為&#x200B;**Just Cash**）。
1. 分割付款區域會顯示在付款方式下方。 現金欄位已預先填入訂購總計，而元件從工作階段讀取&#x200B;**[!UICONTROL Store Credit]**。
1. 對於~$80購物車，設定&#x200B;**$50**&#x200B;現金和&#x200B;**$30**&#x200B;商店點數，元件顯示$50 + $30 = $80。
1. 當金額有效時，請下訂單。

UI會觸發呼叫新的&#x200B;**[!UICONTROL Commerce]** REST API以進行分割。 結帳保持同步： Commerce會套用商店點數、儲存訂單上的分割行，以及發出App Builder稍後會使用的I/O事件。 客戶可立即取得成功頁面。

## 檢閱Commerce管理員中的順序

在&#x200B;**[!UICONTROL Sales]** > **[!UICONTROL Orders]**&#x200B;中，開啟新訂單。 **[!UICONTROL Payment information]**&#x200B;區域顯示分割，例如&#x200B;**$50**&#x200B;現金和&#x200B;**$30**&#x200B;商店點數。 尚未收取現金時狀態為&#x200B;**擱置中**；建立訂單時已取得商店點數。

**[!UICONTROL Comments]**&#x200B;可以包含App Builder新增的文字。 App Builder中的&#x200B;**[!UICONTROL Payment orchestrator action]**&#x200B;已收到I/O事件、檢查分割金額，並使用已驗證的REST附加註解。 **[!UICONTROL Commerce]**&#x200B;會將其視為任何其他REST呼叫，而不需要知道編寫器。

## 使用App Builder操作員儀表板（ERP或後台）

簡單的HTML體驗會以&#x200B;**[!UICONTROL App Builder]** Web動作執行（此檢視沒有任何&#x200B;**[!UICONTROL Admin]**）。 儀表板呼叫&#x200B;**[!UICONTROL Commerce Order]** REST API和篩選器至&#x200B;**待處理**，以便您找到$50的現金分支。

您通常會將此模式與ERP或詐騙工具整合。 示範兩個動作：

* **接受** — 確認已收到現金（在生產中，通常是來自收銀機或商店系統的自動貼文）。
* **拒絕** — 模擬詐騙或失敗的收集步驟（在生產環境中，通常從您的風險服務自動執行）。

**接受並完成訂單**

1. 在&#x200B;**[!UICONTROL App Builder]**&#x200B;應用程式中，使用&#x200B;**Accept**&#x200B;確認現金。
1. 應用程式會呼叫完成現金行的新&#x200B;**[!UICONTROL Commerce]**&#x200B;端點。 核心訂單流程（開立發票，在此組建中自動出貨）會以原生&#x200B;**[!UICONTROL Commerce]**&#x200B;行為執行。 沒有哪個路徑需要在&#x200B;**[!UICONTROL Admin]**&#x200B;中有人操作；協調流程和端點在App Builder中上線。

在&#x200B;**[!UICONTROL Admin]**&#x200B;中重新整理相同的訂單：當現金被接受時，狀態會移至&#x200B;**完成**，並包含商業發票，若產品可出貨，則為縮短示範中的完整生命週期的出貨。

**拒絕並取消**

1. 開啟仍在衰退情境中的預先建立訂單。
1. 在&#x200B;**[!UICONTROL App Builder]**&#x200B;應用程式中，使用&#x200B;**拒絕**&#x200B;來模擬詐騙或集合失敗。
1. 在&#x200B;**[!UICONTROL Admin]**&#x200B;中，訂單為&#x200B;**已取消**。 歷史記錄顯示已拒絕的現金狀態，註解解釋結果，購物者並未將商店銷退折讓明細行當成最終銷售來收費，而商店銷退折讓發票會因取消而失效。 生產系統也會通知客戶，並解除存貨或服務的保留。

## 訂單總計臨界值（建立訂單前的驗證）

App Builder或&#x200B;**[!UICONTROL Commerce]**&#x200B;設定可支援驗證規則；此建置會封鎖購物車值中超過&#x200B;**$100**&#x200B;的訂單（您可變更的示範上限）。 值檢查不是最常見的業務規則 — 詳細目錄或可用性檢查比較典型 — 但它顯示在建立訂單之前，可在&#x200B;**[!UICONTROL Commerce]**&#x200B;中的&#x200B;**[!UICONTROL Payment]**&#x200B;執行驗證，而App Builder仍處理後續的自動化。

1. 新增產品，直到總計超過&#x200B;**$100**&#x200B;為止。
1. 分割&#x200B;**[!UICONTROL UI]**&#x200B;仍會出現；超限結果只會出現在&#x200B;**下訂單**&#x200B;上。
1. 購物者看到一般錯誤，例如&#x200B;**無法處理付款。 請再試一次或連絡支援人員。**
1. **[!UICONTROL cart]**&#x200B;未變更，因此客戶可以降低總計、選擇其他方法或聯絡支援人員。

風險分數、地區規則或類似專案可以插入此處；**[!UICONTROL Commerce]**&#x200B;封鎖訂單，且&#x200B;**[!UICONTROL App Builder]**&#x200B;可以擁有通知，並在事後進行個案工作。

## 內部部署的Commerce、雲端中的Commerce以及&#x200B;**Adobe Commerce as a Cloud Service**

逐步解說符合您管理的基礎結構上的&#x200B;**[!UICONTROL Adobe Commerce]**&#x200B;或傳統店面的&#x200B;**[!UICONTROL Commerce in the cloud]** (PaaS)。 對於前端不同且此形狀沒有處理中模組的&#x200B;**[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS)，App Builder片段大致相同，而店面和擴充功能則會不同。 在所有情況下，相同的原則都成立：讓&#x200B;**[!UICONTROL Commerce]**&#x200B;執行&#x200B;**[!UICONTROL Commerce]**&#x200B;要求內必須進行的動作，並使用&#x200B;**[!UICONTROL App Builder]**&#x200B;處理其餘的體驗。

## 其他資源

* [建立分割付款POC：App Builder和AI工具](create-a-split-payment-poc-app-builder-and-ai-tools.md) — 目標與架構系列簡介。
