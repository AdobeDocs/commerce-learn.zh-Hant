---
title: app.config.yaml檔案
description: 瞭解app.config.yaml檔案如何決定應用程式設定，以及其定義如何連結至Adobe Developer App Builder範例應用程式中的JavaScript檔案。
jira: KT-12929
doc-type: Tutorial
duration: 136
last-substantial-update: 2023-03-13T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
TQID: https://experienceleague.adobe.com/iK4PPaI2-vxQK32DMfkMRZMgNYpLExMbNge2lXIJzLg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: 96
ht-degree: 0%

---

# app.config.yaml檔案的說明和使用方式 {#app-config-yaml}

此檔案決定應用程式的設定。

## 這部影片是給誰看的？

* 剛開始接觸Adobe Commerce、但對Adobe App Builder經驗有限的開發人員，正在範例應用程式中瞭解`app.config.yaml`。

## 視訊內容

* 討論的`app.config.yaml`檔案
* 定義如何連結到其他`.js`個檔案

>[!VIDEO](https://video.tv.adobe.com/v/3416592?learn=on)

## 程式碼範例

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

您可以在檔案`actions/commerce.index.js`中看到這些靜態值正用於範例模組

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
