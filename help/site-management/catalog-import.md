---
title: 瞭解原生Adobe Commerce提供的目錄匯入選項
description: 瞭解如何將目錄匯入Adobe Commerce商店的一些原生選項。
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: b0fe49352b00a68554e662327cd66983c30d8285
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# 匯入目錄的選項

有一些將目錄匯入Adobe Commerce的原生方法。 每種方法都有其使用理由，以及必須考慮的優點和缺點。

從下列其中一個選項選擇以深入瞭解。

>[!BEGINTABS]

>[!TAB 手動]

## 手動建立產品 {#manual-import}

如果您的目錄有限且不常更新，手動建立可能是最佳選擇。 輸入每個產品都需要時間，而且有關如何使用Commerce管理員的訓練也有限。 手動目錄管理對大部分的商店來說不是正確的選項，但在某些情況下，這是合理的。 若要檢視此程式的其他檔案，請造訪 [建立產品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. 別忘了，您可以使用多種方法來管理您的目錄，不過一旦使用自動化，手動編輯必須受到限制。 自動更新有機會覆寫手動執行的任何變更，因此會導致混淆。 在與Adobe Commerce整合以管理目錄使用自動化和API後，建議限制從管理員到管理目錄 [使用者角色和許可權](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.

### 何時考量此方法

- 非常小的目錄，例如少於50種產品
- 不常更新
- 您已擁有所有產品詳細資料、影像、影片，而且您不想花時間瞭解如何將資料轉換為CSV
- 建立產品時要加入新增影像和影片
- 您的團隊為 `not` 熟悉API以及OAUTH的運作方式

>[!TAB 管理員CSV]

## 管理員CSV匯入工具 {#admin-csv}

此工具可讓商店擁有者使用CSV許可權，從商務管理員匯入目錄。
[從Commerce管理員匯入資料](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

優點：從管理員上傳CSV是目錄管理的直接方法。 它允許對適度大小的目錄進行更快的目錄產品更新。

缺點：

- 慢速
- 伺服器上定義的上傳檔案大小上限，商店所有者可能無法輕鬆調整。
- 需要管理員存取權及執行動作的人員，自動化受到限制
- 排程匯入限製為每天最多1次
- 關聯的影像和視訊必須個別上傳

### 何時考量此方法

- 目錄大小為中等
- 更新每天不超過一次
- 您可以存取一些伺服器設定，以備您必須增加檔案上傳大小上限
- 您的團隊為 `not` 熟悉API以及OAUTH的運作方式

>[!TAB 大量REST API]

## 大量REST API {#bulk-rest-api}

大量REST API允許自動化和更頻繁的更新。 此API比使用CSV的管理員上傳更快。
[大量端點檔案](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

優點：匯入不是CSV格式的大型資料集的功能。

缺點：

- 關聯的影像和視訊必須個別上傳
- 可受限於託管提供者的頻寬限制

### 何時考量此方法

- 目錄為任意大小
- 更新頻繁，每天超過1倍是可接受的
- 匯入時間很重要但並不重要，處理匯入資料的短暫延遲是可接受的
- 資料並非以CSV格式結構，且您無法使用自動化將其轉換

>[!TAB 非同步REST API]

## 非同步REST API {#async-rest-api}

非同步Web端點會攔截訊息至Web API，並將其寫入訊息佇列。 每次系統接受這類API請求時，都會產生UUID識別碼。 Adobe Commerce將訊息新增至佇列時，會包含此UUID。 然後，消費者會從佇列中讀取訊息，並逐一執行這些訊息。
[非同步Web端點檔案](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

優點：

- 快速匯入資料
- 支援存放區範圍，或者您可以指定 `all` 若要在所有現有存放區上執行作業

缺點：

- 不支援GET要求

### 何時考量此方法

- 經常匯入
- 從透過API提交並從訊息佇列處理的時間來看，沒有細微的延遲問題。


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

相較於其他所有原生選項，此API選項允許以極快的速度匯入。

[匯入資料REST CSV API](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
優點：

- 處理傳入資料的最快方法
- 可每天執行多次
- 對於大型請求，可以使用gzip來壓縮資料，以避免HTTP請求大小限制。

缺點：

- 關聯的影像和視訊必須個別上傳
- 資料必須是CSV格式

### 何時考量此方法

- 目錄為任意大小
- 更新頻繁，每天超過1倍是可接受的
- 整體匯入時間很重要
- 資料已經是CSV格式，或是可以使用自動化輕鬆轉換

>[!ENDTABS]

## 其他資源

- [使用新的REST CSV匯入資料](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [匯入資料主要檔案](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Adobe Commerce 2.4.6版發行說明](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [使用者、角色和許可權](../site-management/users-roles-permissions.md){target="_blank"}
