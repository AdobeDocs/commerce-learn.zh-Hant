---
title: 安裝Adobe I/O Runtime命令列介面和API Mesh外掛程式
description: 了解如何安裝Adobe I/O Runtime命令列介面和API Mesh外掛程式
landing-page-description: 了解如何使用Adobe應用程式產生器，並使用API Mesh外掛程式安裝Adobe I/O Runtime。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: f365e4cbf3f9bd8a0364acb9d28f8d9479809011
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# 安裝Adobe I/O Runtime CLI和Mesh外掛程式

開始使用Adobe Developer App Builder的API Mesh之前，您必須先安裝 `aio` CLI和API Mesh外掛程式。
如需安裝指示和必要條件，請造訪API Mesh [快速入門](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 頁面。

## 這段錄像是給誰的？

* 剛接觸API Mesh或 [!DNL Adobe Commerce] 使用經驗有限 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 和API Mesh。

## 視訊內容

* API Mesh簡介
* 安裝Adobe I/O Runtime CLI（命令列介面）
* 安裝API Mesh外掛程式

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## 安裝 `aio` CLI和API Mesh外掛程式

安裝後 `node` 和 `npm`，請執行下列命令以安裝 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

安裝Adobe I/O Runtime CLI後，請使用下列命令來安裝API Mesh外掛程式：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
