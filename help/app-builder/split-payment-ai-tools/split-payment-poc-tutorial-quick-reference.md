---
title: 分割付款POC：教學課程快速參考
description: 瞭解分割付款POC檔案對應：哪個Experience League頁面與Luma結帳的每個AI提示、建議的區段順序和作者附註相符。
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 0%

---

# 分割付款POC：教學課程快速參考

此頁面概述如何組織分割付款概念驗證教學課程系列。 它會將每個編號的來源提示檔案對應至其發佈的Experience League頁面，列出讀者建議的區段順序，並收集作者和維護者的備註。


## 逐檔案參考

### [建立分割付款POC：App Builder和AI工具](create-a-split-payment-poc-app-builder-and-ai-tools.md)

**Source標籤：** `00_TUTORIAL_OVERVIEW.md` （概念概觀；發佈的系列會在此頁面開啟。）

**目的：**&#x200B;教學課程的簡介和方向。

**為什麼需要它：**&#x200B;在「如何」之前為開發人員提供「為什麼」。 說明他們將建置什麼、對象是誰，以及如果他們的網站與乾淨的Luma安裝不同，則必須自訂哪些專案。 也可做為其他系列的目錄。

**教學課程使用：**&#x200B;開啟節。 在任何技術步驟之前設定內容。


### [分割付款POC：架構與設計決策](split-payment-poc-architecture-and-decisions.md)


**用途：**&#x200B;深入說明PoC中的每個架構決定。

**為什麼需要它：**&#x200B;使用這個PoC時最常見的困惑點就是瞭解&#x200B;*為什麼* PHP與App Builder之間的差異。 如果沒有此內容，開發人員會嘗試移動無法移動的專案（例如商店信用申請），結果會失敗。 本檔案會以明確的推理來先發制人。

**涵蓋的關鍵主題：**

* 「必須保留在PHP中的內容」規則及原因
* 臨界值雙重執行模式
* 為什麼商店信用申請不能非同步
* 外掛程式處理的五個Commerce邊緣案例
* 擴充功能屬性資料會從→的簽出→報價單→訂單→I/O事件流

**教學課程使用：**「架構」或「運作方式」區段。 經驗豐富的Commerce開發人員可略過，但對於App Builder的新手而言至關重要。


### [分割付款POC：必要條件和環境設定](split-payment-poc-prerequisites-and-setup.md)


**用途：**&#x200B;在寫入任何程式碼或執行提示前，先完成預檢檢查清單。

**為什麼需要：**&#x200B;安裝階段在教學課程中失敗率最高。 本檔案旨在確保Commerce管理員已正確設定（確切的COD標題、已啟用商店點數、測試客戶已取得平衡、已建立整合）、App Builder專案已連線I/O事件和Commerce事件提供者，且本機環境擁有正確的版本。

**強調專案：**

* 「貨到付款」標題必須剛好`Cash` （嚴重 — JavaScript的字串相符）
* Commerce整合必須&#x200B;*已啟用* （不僅已儲存），OAuth認證才能運作
* 這裡說明`entity_id`與`increment_id`，以防止整個過程中發生混淆

**教學課程使用：**「先決條件」或「快速入門」區段。 應該以互動方式完成 — 而不僅僅是閱讀。


### [分割付款POC：環境變數參考](split-payment-poc-env-reference.md)


**用途：**&#x200B;所有三個元件的所有環境變數都集中在一處。

**需要的原因：**&#x200B;相同的四個OAuth認證出現在三個不同檔案中，其變數名稱不同（`COMMERCE_*`與`COMMERCE_INTEGRATION_*`）。 單一參考可防止「為何無法運作」除錯，因為其中有一個認證集含有錯字。

**涵蓋的元件：**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**教學課程使用：**&#x200B;參考側邊欄或「設定」區段。 也可用來輔助建置提示。


### [分割付款POC： Commerce模組AI提示](split-payment-poc-commerce-module-prompt.md)


**用途：**&#x200B;完整、獨立的AI提示，以產生整個`Client_SplitPayment` PHP模組。

**為什麼需要：**&#x200B;這是PHP端的主要AI建置成品。 編寫它是為了讓沒有PHP經驗的開發人員可以將其交給游標（使用Claude）並獲得工作模組。 每個檔案都會指定，每個行為合約都會定義，重要的實作附註也會包含在內，且之後會提供啟用模組的命令。

**涵蓋範圍：**

* 完整的檔案結構（30個以上的檔案）
* 資料庫結構、擴充功能屬性、REST端點、I/O事件設定
* 具有完整行為規格的所有PHP類別（不僅僅是簽名）
* 去底色JS出庫元件規格
* 五個邊緣大小寫外掛程式，並說明每個外掛程式存在的原因
* 非Luma主題的自訂圖說文字

**教學課程使用：**「建置Commerce模組」區段。 提示本身就是成品 — 開發人員將其複製至其AI工具中並加以執行。


### [分割付款POC： App Builder orchestrator AI提示](split-payment-poc-app-builder-orchestrator-prompt.md)


**用途：**&#x200B;完整、獨立的AI提示，可產生`split-payment-orchestrator` App Builder應用程式。

**為什麼需要：**&#x200B;四個App Builder動作(`payment-orchestrator`、`payment-accept`、`payment-decline`、`demo-dashboard`)是「移出PHP的內容」本文的核心。 此提示會以完整的行為規格、正確的`app.config.yaml`結構和事件註冊組態來產生所有這類專案。

**涵蓋範圍：**

* 包含所有四個動作和I/O事件註冊的`app.config.yaml`
* `commerce-client.js` OAuth 1.0a共用使用者端
* 包含完整邏輯規格的全部四個動作
* `store-credit.js`已棄用的Stub （說明&#x200B;*為什麼*&#x200B;它未使用 — 這在教學上很重要）
* 示範控制面板，包含HTML呈現、訂單篩選和安全性
* 包含所有變數的`.env.example`

**教學課程使用：**「建置App Builder應用程式」區段。 Commerce模組提示隨附。


### [分割付款POC： Experience Cloud UI延伸模組AI提示](split-payment-poc-experience-cloud-ui-prompt.md)


**用途：** AI提示以產生選用的Experience Cloud Admin UI SDK擴充功能。

**為什麼需要：** Orchestrator提示中的示範儀表板刻意粗略 — 它是概念證明，而非生產。 本節說明開發人員的下一個步驟：使用管理UI SDK將分割付款控制面板內嵌至Commerce Admin Shell。 原始提示中完全遺失該專案。

**涵蓋範圍：**

* Admin UI SDK擴充功能的`ext.config.yaml`
* 訂單控制面板和訂單詳細資料的React元件
* 後端動作使用IMS驗證來列出清單，使用OAuth來接受/拒絕
* 模擬指令碼（也用於測試）

**教學課程使用：**&#x200B;選用的「更進一步」或「生產路徑」區段。 如果教學課程僅聚焦於PoC，則可略過。


### [分割付款POC：測試與驗證指南](split-payment-poc-testing-and-verification.md)


**用途：**&#x200B;逐步測試指南涵蓋正確驗證順序的每個元件。

**為什麼需要它：**&#x200B;建立元件是工作的一半。 開發人員需要明確的驗證路徑，從最低層級（資料庫欄、REST端點）開始並建置到完整的端對端流程。 原始提示具有設定檢查清單，但沒有明確的測試流程。

**涵蓋範圍：**

* Commerce模組安裝驗證
* 管理員設定驗證
* REST端點煙霧測試（curl命令）
* 結帳UI逐步說明（包括驗證案例）
* 臨界值保護測試
* 完整訂購位置測試
* 接受和拒絕的模擬指令碼使用方式
* 示範控制面板使用情形
* App Builder動作記錄檔檢查
* 十種常見失敗模式及特定修正

**教學課程使用：** 「測試」或「驗證」區段。 也可用作疑難排解參考。


### [分割付款POC：概念證明後的後續步驟](split-payment-poc-next-steps.md)


**用途：**&#x200B;將PoC發展成生產就緒型態的藍圖。

**為什麼需要：** PoC教學課程可能會讓開發人員覺得「現在怎麼辦？」 感覺。 本檔案將說明從示範到生產的自然進展：將示範儀表板取代為Experience Cloud擴充功能、連線真正的ERP、新增API Mesh、擴充App Builder工作流程，以及生產部署檢查清單。

**金鑰內容：**

* 架構發展圖（PoC →階段2→階段3）
* ERP整合模式（哪些會改變，哪些會保持不變）
* API網狀代理程式模式
* 生產部署檢查清單
* 關鍵參考連結

**教學課程使用：**「後續步驟」結束章節。 在設定實際專案的範圍時，也適合作為交談的開始者。


## 建議的教學課程章節

根據這些檔案，讀者的一般結構為：

| 教學課程區段 | Experience League頁面 |
| --- | --- |
| 簡介和學習目標 | [建立分割付款POC：App Builder和AI工具](create-a-split-payment-poc-app-builder-and-ai-tools.md) |
| 端對端示範（影片） | [建立分割付款POC： App Builder完整示範](create-a-split-payment-poc-app-builder-and-ai-tools-full-demo.md) |
| 架構：哪些地方適用 | [分割付款POC：架構與設計決策](split-payment-poc-architecture-and-decisions.md) |
| Prerequisites and setup | [分割付款POC：必要條件和環境設定](split-payment-poc-prerequisites-and-setup.md) |
| Environment variables | [分割付款POC：環境變數參考](split-payment-poc-env-reference.md) |
| Step 1: Build the Commerce module | [分割付款POC： Commerce模組AI提示](split-payment-poc-commerce-module-prompt.md) |
| Step 2: Build the App Builder orchestrator | [分割付款POC： App Builder orchestrator AI提示](split-payment-poc-app-builder-orchestrator-prompt.md) |
| Step 3: Test the end-to-end flow | [分割付款POC：測試與驗證指南](split-payment-poc-testing-and-verification.md) |
| Step 4 (optional): Admin UI extension | [分割付款POC： Experience Cloud UI延伸模組AI提示](split-payment-poc-experience-cloud-ui-prompt.md) |
| Next steps and production path | [分割付款POC：概念證明後的後續步驟](split-payment-poc-next-steps.md) |


## Important notes for tutorial authors

**On the Luma theme assumption:** Every build prompt generates code for a clean Luma installation. The tutorial should clearly state this at the top — developers with custom checkouts will need to adapt the `LayoutProcessorPlugin` injection paths and possibly the RequireJS configuration. The series introduction and build prompts include customization callouts for this.

**On the PHP module:** The tutorial explicitly does not provide the PHP code as a copy-paste download. The AI prompt *generates* it. This is intentional — it teaches the pattern of using AI to create Commerce extensions, not just copy-paste boilerplate. However, the generated code when prompted on a clean environment should match the working code in `app/code/Client/SplitPayment/` exactly.

**On the $100 threshold:** This is a hard-coded PoC value. The tutorial should note that real stores will want to configure this via Commerce Admin configuration rather than a compile-time constant.

**On store credit dependency:** The split payment flow as built requires `Magento_CustomerBalance` (the native store credit module).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
