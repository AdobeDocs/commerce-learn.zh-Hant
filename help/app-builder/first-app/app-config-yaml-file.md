---
title: app.config.yaml檔案
description: 了解此範例應用程式的app.config.yaml檔案中的檔案類型。
landing-page-description: 了解與Adobe Commerce搭配使用的Adobe Developer App Builder，以及app.config.yaml中會傳入哪些類型的檔案。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: e0371a8cefab0141318daa0e1be42bfbb9e5b608
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---


# app.config.yaml檔案的說明和用法 {#app-config-yaml}

此檔案決定應用程式的設定。

## 這段錄像是給誰的？

* 剛接觸Adobe Commerce的開發人員，在Adobe應用程式建立工具上擁有的體驗有限，他們了解 `app.config.yaml` 在範例應用程式中。

## 視訊內容

* 此 `app.config.yaml` 討論的檔案
* 定義如何連結至其他 `.js` 檔案

>[!VIDEO](https://video.tv.adobe.com/v/3416592)

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

您可以在檔案的範例模組中看到這些靜態值正在使用 `actions/commerce.index.js`

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
