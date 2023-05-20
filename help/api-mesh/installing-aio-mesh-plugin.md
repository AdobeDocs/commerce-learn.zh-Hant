---
title: 安裝Adobe I/O Runtime命令行介面和API Mesh插件
description: 瞭解如何安裝Adobe I/O Runtime命令行介面和API Mesh插件
landing-page-description: 瞭解如何使用AdobeApp Builder和使用API Mesh插件安裝Adobe I/O Runtime。
short-description: 瞭解如何使用AdobeApp Builder和使用API Mesh插件安裝Adobe I/O Runtime。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# 安裝Adobe I/O RuntimeCLI和Mesh插件

在開始為Adobe Developer應用生成器使用API Mesh之前，需要安裝 `aio` CLI和API Mesh插件。
有關安裝說明和先決條件，請訪問API Mesh [入門](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 的子菜單。

## 這段錄像是給誰的？

* API Mesh或 [!DNL Adobe Commerce] 經驗有限 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 和API Mesh。

## 視頻內容

* API Mesh簡介
* 安裝Adobe I/O RuntimeCLI（命令行介面）
* 安裝API Mesh插件

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## 安裝 `aio` CLI和API Mesh插件

安裝後 `node` 和 `npm`，運行以下命令以安裝 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

安裝Adobe I/O RuntimeCLI後，請使用以下命令安裝API Mesh插件：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
