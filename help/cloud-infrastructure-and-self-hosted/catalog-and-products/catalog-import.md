---
title: Adobe Commerce中的原生目錄匯入選項
description: 瞭解如何將目錄匯入Adobe Commerce商店的一些原生選項。
jira: KT-13634
doc-type: Tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-08-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
TQID: 'https://experienceleague.adobe.com/-JG7blrxImSXjA2DP9soZfsicISW0hkP2zJeWdMpVBU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: f56065aac7f0888edb163a6c41da3481c166cc06
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%
---
# 匯入目錄的選項

有一些將目錄匯入Adobe Commerce的原生方法。 每種方法都有其使用理由，以及必須考慮的優點和缺點。

從下列其中一個選項選擇以深入瞭解。

>[!BEGINTABS]

>[!TAB 手動]

## 手動建立產品 {#manual-import}

考慮以手動建立產品方式，建立不常更新的小型目錄。 輸入每個產品都需要一些時間，並且需要有關Commerce管理員的基本培訓。 手動目錄管理適合特定情況，而非大多數商店。 如需指示，請參閱[建立產品](https://experienceleague.adobe.com/en/docs/commerce-admin/catalog/products/product-create){target="_blank"}。 如果您結合目錄管理方法，請在引入自動化後限制手動編輯。 自動更新可能會覆寫手動變更並導致混淆。 當整合透過自動化和API管理目錄時，請透過[使用者角色和許可權](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/user-accounts/permissions-user-roles){target="_blank"}限制管理員中的目錄管理。

### 何時考量此方法

* 非常小的目錄，不到50種產品
* 不常更新
* 您已擁有所有產品詳細資料、影像、影片，而且您不想花時間瞭解如何將資料轉換為CSV
* 建立產品時要加入新增影像和影片
* 您的團隊`not`熟悉API以及OAUTH的運作方式

>[!TAB 管理員CSV]

## 管理員CSV匯入工具 {#admin-csv}

此工具可讓商店擁有者使用CSV許可權，從商務管理員匯入目錄。
[從Commerce管理員匯入資料](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/import/data-import){target="_blank"}

優點：
從管理員上傳CSV是目錄管理的直接方法。 它允許對適度大小的目錄進行更快的目錄產品更新。

缺點：

* 慢速
* 伺服器會定義上傳檔案大小上限，而商店所有者並不總是能夠輕鬆進行調整。
* 需要管理員存取權及執行動作的人員，自動化受到限制
* 排程匯入限製為每天最多1次
* 關聯的影像和視訊必須個別上傳

### 何時考量此方法

* 目錄大小為中等
* 更新每天不超過一次
* 如果您需要增加上傳檔案大小上限，則可存取伺服器設定
* 您的團隊`not`熟悉API以及OAUTH的運作方式

>[!TAB 大量REST API]

## 大量REST API {#bulk-rest-api}

大量REST API允許自動化和更頻繁的更新。 此API比使用CSV的管理員上傳更快。
[大量端點檔案](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints){target="_blank"}

優點：
匯入不是CSV格式的大型資料集的功能。

缺點：

* 關聯的影像和視訊必須個別上傳
* 主控提供者的頻寬限制會限制匯入輸送量

### 何時考量此方法

* 目錄為任意大小
* 更新頻繁，每天超過1倍是可接受的
* 匯入時間很重要但並不重要，處理匯入資料的短暫延遲是可接受的
* 資料並非以CSV格式結構，且您無法使用自動化將其轉換

>[!TAB 非同步REST API]

## 非同步REST API {#async-rest-api}

非同步Web端點會攔截訊息至Web API，並將其寫入訊息佇列。 每次系統接受這類API請求時，都會產生UUID識別碼。 Adobe Commerce將訊息新增至佇列時，會包含此UUID。 然後，消費者會從佇列中讀取訊息，並逐一執行這些訊息。
[非同步Web端點檔案](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints){target="_blank"}

優點：

* 快速匯入資料
* 支援存放區範圍，或者您可以指定`all`在所有現有存放區上執行作業

缺點：

* 不支援GET要求

### 何時考量此方法

* 經常匯入
* 從透過API提交並從訊息佇列處理的時間來看，沒有細微的延遲問題。


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

此API匯入資料的速度比其他原生選項更快。

[匯入資料REST CSV API](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
優點：

* 處理傳入資料的最快方法
* 可每天執行多次
* 對於大型請求，可以使用gzip來壓縮資料，以避免HTTP請求大小限制。

缺點：

* 關聯的影像和視訊必須個別上傳
* 資料必須是CSV格式

### 何時考量此方法

* 目錄為任意大小
* 更新頻繁，每天超過1倍是可接受的
* 整體匯入時間很重要
* 資料已經是CSV格式，或是可以使用自動化輕鬆轉換

>[!ENDTABS]

## 其他資源

* [使用新的REST CSV匯入資料](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
* [匯入資料主要檔案](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/import/data-import){target="_blank"}
* [Adobe Commerce 2.4.6版發行說明](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-6){target="_blank"}
* [使用者、角色和許可權](../store-administration/users-roles-permissions.md){target="_blank"}
