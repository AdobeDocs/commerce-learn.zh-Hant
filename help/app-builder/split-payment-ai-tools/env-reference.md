---
title: 分割付款POC：環境變數參考
description: 瞭解如何將Commerce OAuth、基本URL、付款臨界值和選用的示範設定對應到Orchestrator、UI擴充功能和模擬環境檔案。
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: d5f1e76c3a5127698f2933810fca218b79082571
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 分割付款POC：環境變數參考

每個元件都會使用相同的四個Commerce OAuth憑證。 在&#x200B;**[!UICONTROL Commerce Admin]**&#x200B;中建立一個&#x200B;**[!UICONTROL Integration]**，然後重複使用下面每個`.env`檔案中的四個值。 （如需啟用步驟，請參閱[分割付款POC：必要條件和環境設定](./prerequisites-and-setup.md)。）

## 四個OAuth認證（適用於所有地方）

| 變數 | 從何處取得 |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[您的整合]* |
| `COMMERCE_CONSUMER_SECRET` | 與上述相同 — 值僅在啟動時顯示 |
| `COMMERCE_ACCESS_TOKEN` | 與上述相同 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 與上述相同 |


## App Builder orchestrator

### `split-payment-orchestrator/.env`

從orchestrator目錄中的`.env.example`複製。 請勿認可此檔案。

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Experience Cloud UI擴充功能(commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

此元件使用兩個認證集：IMS用於使用&#x200B;**[!UICONTROL Admin]** UI SDK的訂單清單，而OAuth 1.0a用於接受和拒絕動作。

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## 模擬指令碼

### `commerce-backend-ui-1/.env.simulation`

從相同目錄中的`.env.simulation.example`複製。

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## 附註

**`PAYMENT_THRESHOLD`** — 必須符合&#x200B;**[!UICONTROL Commerce]**&#x200B;系統組態中的`split_payment/general/threshold`。 如果值遺失、非數值或小於或等於`0`，則兩側預設為`100`。 如果您在&#x200B;**[!UICONTROL Commerce]**&#x200B;中變更臨界值，請更新App Builder `.env`以符合。

**`DEMO_UI_SECRET`** — 選擇性，但建議用於任何非localhost的部署。 擁有儀表板URL的任何人都可以列出訂單，並在空白時執行「接受」和「拒絕」。 若是實際的測試環境，請設定共用機密。

**`COMMERCE_BASE_URL`** — 絕對不要在結尾加上斜線。 Commerce REST使用者端會自動附加`/rest/V1/`。

**`AIO_CLI_ENV`** — 為&#x200B;**[!UICONTROL Stage]**&#x200B;工作區設定為`stage`。 部署至&#x200B;**[!UICONTROL Production]**&#x200B;時變更為`prod`。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
