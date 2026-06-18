---
title: 安裝Adobe I/O Runtime CLI和API Mesh外掛程式
description: 瞭解如何安裝Adobe I/O Runtime命令列介面和API Mesh外掛程式，以開始使用Adobe Developer App Builder的API Mesh。
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# 安裝Adobe I/O Runtime CLI和Mesh外掛程式

開始使用Adobe Developer App Builder的API Mesh之前，您必須安裝`aio` CLI和API Mesh外掛程式。
如需安裝指示和先決條件，請造訪API Mesh [快速入門](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"}頁面。

## 這部影片是給誰看的？

* 剛開始使用API Mesh或使用[Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}和API Mesh且經驗有限的[!DNL Adobe Commerce]開發人員。

## 視訊內容

* API Mesh簡介
* 安裝Adobe I/O Runtime CLI （命令列介面）
* 安裝API網狀外掛程式

>[!VIDEO](https://video.tv.adobe.com/v/3430769?captions=chi_hant&learn=on)

## 安裝`aio` CLI和API Mesh外掛程式

若要安裝`aio` CLI，請在安裝`node`和`npm`後執行下列命令：

```bash
npm install -g @adobe/aio-cli
```

安裝Adobe I/O Runtime CLI後，請使用下列命令來安裝API Mesh外掛程式：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
