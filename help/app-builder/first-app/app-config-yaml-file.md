---
title: app.config.yaml檔案
description: 瞭解此示例應用程式的app.config.yaml檔案中的檔案類型。
landing-page-description: 瞭解與Adobe Commerce一起使用的Adobe Developer應用程式生成器以及app.config.yaml中的檔案類型。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# app.config.yaml檔案的說明和使用 {#app-config-yaml}

此檔案確定應用程式的配置。

## 這段錄像是給誰的？

* 新進Adobe Commerce的開發人員，在Adobe應用構建器方面經驗有限，他們正在瞭解 `app.config.yaml` 的下界。

## 視頻內容

* 的 `app.config.yaml` 已討論
* 定義如何連結到其他 `.js` 檔案

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

## 代碼示例

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

您可以看到這些靜態值正在檔案的示例模組中使用 `actions/commerce.index.js`

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
