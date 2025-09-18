---
title: 如何查詢資料
description: 瞭解如何查詢Adobe Commerce Optimizer執行個體的資料。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
source-git-commit: 5d34c2e3b93c937139e88fa2f75dc6046f7093fc
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 查詢資料Adobe Commerce Optimizer

瞭解如何在Adobe Commerce Optimizer執行個體上使用GraphQL查詢資料。

## 這部影片是給誰看的？

* Commerce解決方案架構師與開發人員

## 視訊內容

* 使用GraphQL查詢資料
* 使用jq讓json更易於閱讀

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

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

* [銷售API快速入門](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/zh-hant/docs/commerce/optimizer/overview){target="_blank"}
