---
title: 探索新的客戶REST API
description: 瞭解如何在Adobe Commerce雲端服務中使用新的客戶REST API。 是架構師與開發人員的理想選擇。
feature: REST, Customers, Saas
topic: Development, Integrations
role: Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2026-01-27T00:00:00.000Z
jira: KT-20160
exl-id: f40d9b21-1f41-4c76-84a9-161168dbfb1a
TQID: https://experienceleague.adobe.com/DiP21e4T-iLM-IuOVDVkJIvHOJ6y-q4IIdSKVplxcX0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: bd989d82-1e15-4534-88db-f1f51dd77ffaid: c32adafa-ed01-4b31-997e-2413013911b0id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 505
ht-degree: 0%

---

# 客戶重設API

瞭解如何在Adobe Commerce as a Cloud Service中使用新的客戶REST API。 此教學課程非常適合想要有效整合及最佳化API解決方案的架構師和開發人員。

## 這部影片是給誰看的？

* 負責建置Adobe Commerce整合功能的後端開發人員
* 技術架構師為Headless商務實施設計客戶管理工作流程

## 視訊內容

* 使用伺服器對伺服器憑證向Adobe IMS進行驗證以取得API請求的存取權杖
* 對Commerce as a Cloud Service使用正確的REST API端點格式
* 以程式設計方式使用POST和PUT請求，搭配適當的JSON負載，建立並更新客戶帳戶

>[!VIDEO](https://video.tv.adobe.com/v/3479361?learn=on)

## 程式碼範例

開始之前，請先從[Experience Cloud](https://experience.adobe.com)和[Adobe Developer Console](https://developer.adobe.com/console)收集所有必要的值。 準備好這些值可確保順利的設定程式。

>[!NOTE]
>
>確定您是在正確的組織內工作。 您的組織選擇會影響哪些例項和環境可在Experience Cloud和Developer Console中看到。

### 執行個體詳細資訊 — experience.adobe.com

執行個體詳細資料包含您的執行個體ID、GraphQL端點、憑證等內容。

### 開發人員詳細資訊 — https://developer.adobe.com/console/

Developer Console是您管理API憑證的地方，包括使用者端ID、使用者端密碼和存取權杖。 您也可以建立新的認證型別，例如伺服器對伺服器或原生應用程式驗證。

## 先決條件

| 專案 | 值 | 其中是此值 |
|--- |--- |--- |
| 執行個體ID | `<instance_id>` | experience.adobe.com |
| REST端點 | `<rest_endpoint>` | experience.adobe.com |
| 使用者端ID | `<client_id>` | developer.adobe.com/console |
| 使用者端密碼 | `<client_secret>` | developer.adobe.com/console |


## 步驟1：取得存取Token （伺服器對伺服器驗證）

>[!IMPORTANT]
>
> 此範例中所示的變數無效。 使用專案認證中的&lt;client_id>和&lt;client_secret>。

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**範例回應：**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## 步驟2：建立客戶

>[!IMPORTANT]
>
> 此範例提供的URL無效。 使用您的REST基底url。 將「&lt;rest_endpoint>」與您的URL交換。 它看起來類似於此`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`。
>
> 此端點沒有/rest/做為URL的一部分。 包括此專案會導致錯誤。

**端點：** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**回應：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## 步驟3：更新客戶

>[!IMPORTANT]
>
> 此範例提供的URL無效。 使用您的REST基底url。 將「&lt;rest_endpoint>」與您的URL交換。 它看起來類似於此`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`。

The number `5` in the following example is the ID from the previously created customer using POST `"id": 5,`. Be sure to change`5` to whatever id was returned in your request.

**端點：** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**回應：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## Complete script (all-in-one)

>[!IMPORTANT]
>
> 此範例中所示的變數無效。 Use the client ID and client secret from your project credentials. 使用您的REST基底url。 Exchange &#39;&lt;rest_endpoint>&#39; with your REST endpoint URL from experience.adobe.com. It looks similar to this  `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## Important notes about this tutorial

1. **URL Path**: Use `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **Authentication**: This tutorial used Server-to-Server (`client_credentials` grant type)
1. **Required Scope**: `commerce.accs`
1. **Token Expiry**: 86400 seconds (24 hours)

## References

* [Adobe Commerce as a Cloud Service Release Notes](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [SaaS REST API Reference](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [User Authentication Guide](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
