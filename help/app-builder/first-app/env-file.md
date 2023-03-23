---
title: .env檔案
description: 了解此範例應用程式的.env檔案中的檔案類型
landing-page-description: 了解與Adobe Commerce搭配使用的Adobe Developer App Builder，以及.env檔案中使用的內容類型
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: e0371a8cefab0141318daa0e1be42bfbb9e5b608
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# 產生並設定.env檔案 {#env-file}

此 `.env` 是特殊檔案，不屬於範例模組，但在Adobe Developer App Builder應用程式中請務必使用。 此檔案包含機密和其他資訊。 請避免將此檔案提交至任何程式碼存放庫。

## 這段錄像是給誰的？

* 剛接觸Adobe Commerce的開發人員，使用Adobe應用程式產生器的體驗有限，且想了解 `.env` 檔案。

## 視訊內容

* .env檔案簡介及其用途
* 如何產生.env檔案
* 如何附加檔案以添加新機密
* 請避免提交此檔案，因為它包含敏感資訊

>[!VIDEO](https://video.tv.adobe.com/v/3416593)

## 程式碼範例

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

您可以在檔案的範例模組中看到這些靜態值正在使用 `actions/commerce.index.js`.

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
