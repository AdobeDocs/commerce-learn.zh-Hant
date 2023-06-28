---
title: app.config.yaml檔案
description: 瞭解此範例應用程式的app.config.yaml檔案中的檔案型別。
landing-page-description: 瞭解搭配Adobe Commerce使用的Adobe Developer App Builder以及app.config.yaml中的檔案型別。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# app.config.yaml檔案的說明和使用方式 {#app-config-yaml}

此檔案決定應用程式的設定。

## 這部影片是給誰看的？

* 剛開始使用Adobe Commerce且在AdobeApp Builder方面經驗有限的開發人員，他們現在正在瞭解 `app.config.yaml` 在範例應用程式中。

## 視訊內容

* 此 `app.config.yaml` 已討論的檔案
* 定義如何連結至其他 `.js` 檔案

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

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

您可以在檔案中看到範例模組使用這些靜態值 `actions/commerce.index.js`

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
