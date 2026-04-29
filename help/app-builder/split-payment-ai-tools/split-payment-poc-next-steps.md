---
title: 分割付款POC：概念證明後的後續步驟
description: 瞭解如何將分割付款POC移至生產環境：Experience Cloud UI、ERP鉤點、API Mesh、PHP範圍、App Builder工作流程以及部署檢查清單。
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# 分割付款POC：概念證明後的後續步驟

您在本教學課程中建置的示範控制面板和模擬指令碼會刻意粗略。 它們存在是為了證明此模式。 本頁說明從概念證明到生產樣式App Builder開發的實用途徑。


## 步驟1 — 以Experience Cloud UI擴充功能取代示範控制面板

`demo-dashboard` Web動作會從Node.js函式內的字串提供HTML。 這很管用，但不是生產模式。

適當的取代專案是`commerce-checkout-starter-kit`中的`commerce-backend-ui-1`擴充功能 — React應用程式，透過Adobe管理UI SDK內嵌於Commerce管理命令介面。 如需產生提示，請參閱[分割付款POC：Experience Cloud UI擴充功能AI提示](split-payment-poc-experience-cloud-ui-prompt.md)。

**哪些變更：**
* 操作員透過Commerce管理員殼層登入（以IMS驗證而非共用密碼）
* 訂單清單使用IMS權杖內容 — 不需要示範密碼
* 接受/拒絕動作的範圍限定於運運算元的IMS身分
* UI內嵌於Commerce管理員中，位於Commerce管理員已知的URL

**維持不變的專案：**
* `payment-accept`和`payment-decline`個App Builder動作未變更
* Commerce REST端點未變更
* PHP模組未變更


## 步驟2 — 連線真正的ERP

此PoC中的接受/拒絕流程是由使用者按一下按鈕所驅動。 在生產環境中，這會由您的ERP、CRM或支付處理器驅動。

**整合模式：**
1. 您的ERP系統會使用`{ orderId: <entity_id> }`擷取現金並呼叫`POST /payment-accept` （App Builder網頁動作URL）
2. App Builder validates the call (bearer token or API key — add auth middleware to `payment-accept`)
3. App Builder calls Commerce REST `cash-received`
4. Commerce invoices and ships the order

No PHP changes required. The REST surface is already there. The App Builder action just needs a secure caller instead of a dashboard button.

**Authentication options for the ERP caller:**
* Adobe I/O Runtime supports `require-adobe-auth: true` for IMS tokens (if your ERP can get an IMS token)
* For non-Adobe systems: add a simple API key check in the `payment-accept` action (check a header against a secret stored in the action&#39;s env)


## Step 3 — Add API Mesh as a Broker Layer

Currently, App Builder calls Commerce REST directly with OAuth 1.0a credentials. For larger deployments, Adobe API Mesh provides a managed gateway layer between App Builder and Commerce.

**Benefits:**
* Centralized credential management — API Mesh holds the Commerce credentials; App Builder calls API Mesh
* Request transformation — map App Builder payloads to Commerce REST shapes without changing the App Builder action
* Rate limiting and caching — protect Commerce from high-volume event traffic

**The pattern:**
* Replace `createCommerceClient(params, logger)` calls with calls to your API Mesh endpoint
* API Mesh handles OAuth signing toward Commerce


## Step 4 — Reduce the PHP Footprint

The current PHP module handles five things that must stay in-process (see [Split payment POC: architecture and design decisions](split-payment-poc-architecture-and-decisions.md)). As Adobe Commerce&#39;s API surface matures, some of these may become movable:

**Potentially movable in the future:**
* The store credit REST API is evolving — future versions may support applying credit post-order or to inactive carts
* As more Commerce operations become async-safe, the threshold guard may be movable to a pre-order API Mesh resolver

**不可移動（架構限制）：**
* 除非Commerce透過API優先擴充點公開乾淨的勾點，否則`placeOrder()`之前的引號操作一律需要處理中的程式碼
* REST端點(`/V1/split-payment/*`)專屬於此功能；它們存在於Commerce中，因為它們呼叫Commerce內部服務


## 步驟5 — 新增更多工作流程至App Builder

目前的PoC會執行一項作業：接聽訂單位置，然後啟用「接受/拒絕」。 自然擴充功能：

接受前&#x200B;**欺詐評分：**
在`payment-orchestrator`中，在記錄待處理現金後，請先呼叫詐騙評分API，再將協調結果視為最終結果。 如果分數高於臨界值，則會自動拒絕，而非等待運運算元動作。

**通知電子郵件：**
當`payment-accept`成功時，觸發電子郵件（透過Adobe Campaign、SendGrid或任何HTTPS API）通知客戶其現金付款已確認。

**忠誠度點數獎勵：**
現金確認後，請致電忠誠度API以獎勵積分。 這是純粹的App Builder — 不需要PHP。

**逾時處理：**
新增排程的App Builder動作（使用`app.config.yaml`中的`cron`），該動作會掃描超過X天的`split_cash_status = 'pending'`訂單並自動拒絕它們。


## 步驟6 — 部署到生產

已為`Stage`工作區設定PoC。 移至生產環境：

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**生產準備檢查清單：**
* [ 已設定] `DEMO_UI_SECRET` （或已使用Experience Cloud UI取代示範儀表板）
* [ 生產環境中的] `LOG_LEVEL=warn`或`error` （不是`debug`）
* [ ] `PAYMENT_THRESHOLD`符合Commerce生產設定
* [ `.env`中的] Commerce整合認證是用於專用的生產整合（非測試）
* [ ]個Fastly IP允許清單已使用App Builder生產輸出IP更新(Commerce Cloud)
* [ 已在生產工作區中確認]個I/O事件註冊


## 架構發展圖

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## 重要參考資料

* [Adobe App Builder檔案](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [適用於Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce Checkout入門套件](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [Adobe管理UI SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Adobe API Mesh](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime (OpenWhise)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
