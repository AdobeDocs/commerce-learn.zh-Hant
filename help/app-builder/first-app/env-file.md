---
title: .env檔案
description: 瞭解此示例應用程式的.env檔案中的檔案類型
landing-page-description: 瞭解與Adobe Commerce一起使用的Adobe Developer應用程式生成器以及.env檔案中使用的內容類型
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 生成並配置.env檔案 {#env-file}

的 `.env` 是一個特殊檔案，它不是示例模組的一部分，但對於在您的Adobe DeveloperApp Builder應用程式中使用非常重要。 此檔案包含機密和其他資訊。 避免將此檔案提交到任何代碼儲存庫。

## 這段錄像是給誰的？

* 剛進入Adobe Commerce的開發人員，在使用Adobe應用程式構建器方面經驗有限，他們希望瞭解 `.env` 的子菜單。

## 視頻內容

* .env檔案簡介及其用途
* 如何生成.env檔案
* 如何追加檔案以添加新機密
* 避免提交此檔案，因為它包含敏感資訊

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

## 代碼示例

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

您可以看到這些靜態值正在檔案的示例模組中使用 `actions/commerce.index.js`。

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
