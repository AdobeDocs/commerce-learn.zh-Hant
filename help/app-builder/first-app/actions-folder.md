---
title: 動作資料夾
description: 瞭解此範例應用程式actions資料夾中的檔案型別。
landing-page-description: 瞭解搭配Adobe Commerce使用的Adobe Developer App Builder以及動作資料夾中的檔案型別。
kt: 12422
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: d13ed1e7-b18e-4bf5-af87-2a69e2588d65
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 瞭解動作資料夾 {#actions-folder}

這個範例應用程式的`actions`資料夾包含一些JavaScript檔案和一個名為`commerce`的資料夾。 顯示的JavaScript是極佳的範例檔案，如果您的工作與檔案相關，則可重複使用。 使用OAuth和REST連線至Adobe Commerce應用程式時，此資料夾可為您節省開發時間。

此範例中，資料夾的實際名稱是任意的，但知道其名稱可協助您解譯範常式式碼。 透過使用有意義的命名慣例，可以避免在應用程式變得較為複雜時造成混淆。

## 這部影片是給誰看的？

* 剛開始接觸Adobe Commerce但AdobeApp Builder經驗有限的開發人員，瞭解範例應用程式中的動作資料夾。

## 視訊內容

* 介紹App Builder和專注於`actions`資料夾的範例模組
* 如何使用「actions」資料夾
* 在`actions`和`commerce`資料夾中找到的JavaScript檔案用途
* OAuth驗證檔案的快速總覽

>[!VIDEO](https://video.tv.adobe.com/v/3416590?quality=12&learn=on)

## 程式碼範例

actions/oauth1a.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, checkMissingRequestInputs} = require('../utils')
const { getCommerceOauthClient } = require('../oauth1a')

async function main(params) {
    const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

    try {
        const requiredParams = ['operation', 'COMMERCE_BASE_URL']
        const requiredHeaders = ['Authorization']
        const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
        if (errorMessage) {
            // return and log client errors
            return errorResponse(400, errorMessage, logger)
        }

        const { operation } = params

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

        const content = await oauth.get(operation)

        return {
            statusCode: 200,
            body: content
        }
    } catch (error) {
        // log any server errors
        logger.error(error)
        // return with 500
        return errorResponse(500, error, logger)
    }
}

exports.main = main
```

actions/utils.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */

/* This file exposes some common utilities for your actions */

/**
 *
 * Returns a log ready string of the action input parameters.
 * The `Authorization` header content will be replaced by '<hidden>'.
 *
 * @param {object} params action input parameters.
 *
 * @returns {string}
 *
 */
function stringParameters (params) {
  // hide authorization token without overriding params
  let headers = params.__ow_headers || {}
  if (headers.authorization) {
    headers = { ...headers, authorization: '<hidden>' }
  }
  return JSON.stringify({ ...params, __ow_headers: headers })
}

/**
 *
 * Returns the list of missing keys giving an object and its required keys.
 * A parameter is missing if its value is undefined or ''.
 * A value of 0 or null is not considered as missing.
 *
 * @param {object} obj object to check.
 * @param {array} required list of required keys.
 *        Each element can be multi level deep using a '.' separator e.g. 'myRequiredObj.myRequiredKey'
 *
 * @returns {array}
 * @private
 */
function getMissingKeys (obj, required) {
  return required.filter(r => {
    const splits = r.split('.')
    const last = splits[splits.length - 1]
    const traverse = splits.slice(0, -1).reduce((tObj, split) => { tObj = (tObj[split] || {}); return tObj }, obj)
    return traverse[last] === undefined || traverse[last] === '' // missing default params are empty string
  })
}

/**
 *
 * Returns the list of missing keys giving an object and its required keys.
 * A parameter is missing if its value is undefined or ''.
 * A value of 0 or null is not considered as missing.
 *
 * @param {object} params action input parameters.
 * @param {array} requiredHeaders list of required input headers.
 * @param {array} requiredParams list of required input parameters.
 *        Each element can be multi level deep using a '.' separator e.g. 'myRequiredObj.myRequiredKey'.
 *
 * @returns {string} if the return value is not null, then it holds an error message describing the missing inputs.
 *
 */
function checkMissingRequestInputs (params, requiredParams = [], requiredHeaders = []) {
  let errorMessage = null

  // input headers are always lowercase
  requiredHeaders = requiredHeaders.map(h => h.toLowerCase())
  // check for missing headers
  const missingHeaders = getMissingKeys(params.__ow_headers || {}, requiredHeaders)
  if (missingHeaders.length > 0) {
    errorMessage = `missing header(s) '${missingHeaders}'`
  }

  // check for missing parameters
  const missingParams = getMissingKeys(params, requiredParams)
  if (missingParams.length > 0) {
    if (errorMessage) {
      errorMessage += ' and '
    } else {
      errorMessage = ''
    }
    errorMessage += `missing parameter(s) '${missingParams}'`
  }

  return errorMessage
}

/**
 *
 * Extracts the bearer token string from the Authorization header in the request parameters.
 *
 * @param {object} params action input parameters.
 *
 * @returns {string|undefined} the token string or undefined if not set in request headers.
 *
 */
function getBearerToken (params) {
  if (params.__ow_headers &&
      params.__ow_headers.authorization &&
      params.__ow_headers.authorization.startsWith('Bearer ')) {
    return params.__ow_headers.authorization.substring('Bearer '.length)
  }
  return undefined
}
/**
 *
 * Returns an error response object and attempts to log.info the status code and error message
 *
 * @param {number} statusCode the error status code.
 *        e.g. 400
 * @param {string} message the error message.
 *        e.g. 'missing xyz parameter'
 * @param {*} [logger] an optional logger instance object with an `info` method
 *        e.g. `new require('@adobe/aio-sdk').Core.Logger('name')`
 *
 * @returns {object} the error object, ready to be returned from the action main's function.
 *
 */
function errorResponse (statusCode, message, logger) {
  if (logger && typeof logger.info === 'function') {
    logger.info(`${statusCode}: ${message}`)
  }
  return {
    error: {
      statusCode,
      body: {
        error: message
      }
    }
  }
}

module.exports = {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs
}
```

actions/commerce/index.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, checkMissingRequestInputs} = require('../utils')
const { getCommerceOauthClient } = require('../oauth1a')

async function main(params) {
    const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

    try {
        const requiredParams = ['operation', 'COMMERCE_BASE_URL']
        const requiredHeaders = ['Authorization']
        const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
        if (errorMessage) {
            // return and log client errors
            return errorResponse(400, errorMessage, logger)
        }

        const { operation } = params

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

        const content = await oauth.get(operation)

        return {
            statusCode: 200,
            body: content
        }
    } catch (error) {
        // log any server errors
        logger.error(error)
        // return with 500
        return errorResponse(500, error, logger)
    }
}

exports.main = main
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
