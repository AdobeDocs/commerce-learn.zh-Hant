---
title: .env檔案
description: 瞭解此範例應用程式的.env檔案中的檔案型別
landing-page-description: 瞭解搭配Adobe Commerce使用的Adobe Developer App Builder以及.env檔案中使用的內容型別
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 產生並設定.env檔案 {#env-file}

`.env`是特殊檔案，不屬於範例模組，但對於Adobe Developer App Builder應用程式中的使用很重要。 此檔案包含密碼和其他資訊。 避免將此檔案提交至任何程式碼存放庫。

## 這部影片是給誰看的？

* 剛開始使用Adobe Commerce但使用AdobeApp Builder經驗有限的開發人員，他們想要瞭解`.env`檔案。

## 視訊內容

* .env檔案的簡介及其用途
* 如何產生.env檔案
* 如何附加檔案以新增密碼
* 避擴音交此檔案，因為它包含敏感資訊

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

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

您可以在檔案`actions/commerce.index.js`中看到這些靜態值正用於範例模組。

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
