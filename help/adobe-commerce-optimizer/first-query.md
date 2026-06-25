---
title: 如何查詢資料
description: 瞭解如何使用GraphQL查詢Adobe Commerce Optimizer產品資料，包括如何使用jq和結構搜尋查詢變數來格式化JSON回應。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 204
last-substantial-update: 2025-08-13
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
TQID: https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 127
ht-degree: 0%

---

# 查詢資料Adobe Commerce Optimizer

瞭解如何在Adobe Commerce Optimizer執行個體上使用GraphQL查詢資料。

## 這部影片是給誰看的？

* Commerce解決方案架構師與開發人員

## 視訊內容

* 使用GraphQL查詢資料
* 使用jq讓json更易於閱讀

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on)

## 程式碼範例

請確定將`{{insert-your-graphql-endpoint-url}}`、`{{insert-your-ac-view-id}}`和`{{your-search-query-string}}`等值與查詢所需的值交換。

基本範例查詢

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

使用`jq`以美化列印輸出的基本範例查詢

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 相關內容

* [開始使用Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/zh-hant/docs/commerce/optimizer/overview){target="_blank"}

