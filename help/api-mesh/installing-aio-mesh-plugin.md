---
title: 安裝Adobe Developer IO命令列介面和API Mesh外掛程式
description: 了解如何安裝Adobe Developer IO命令列介面和API Mesh外掛程式
landing-page-description: 了解如何使用Adobe應用程式產生器，並安裝具有API Mesh外掛程式的Adobe Developer IO。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 安裝Adobe Developer IO和Mesh外掛程式

開始之前，有一些需要設定的項目。 首先，設定Adobe Developer IO命令行介面。 接下來，請確定已在每個環境中設定API Mesh外掛程式。
有關設定本地環境以運行Node、nvm和安裝Adobe Developer IO的說明，請務必訪問 [GraphQL Mesh快速入門](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## 這段錄像是給誰的？

* 剛AdobeApp Builder或 [!DNL Magento Open Source] 在Adobe Developer IO和API Mesh方面經驗有限。

## 視訊內容

* API Mesh簡介
* 安裝Adobe Developer IO命令列介面
* 將API Mesh外掛程式新增至AIO命令列

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## 使用NPM和AIO的命令範例

安裝Adobe Developer命令列介面相當簡單。 安裝Node後，運行此命令 `npm install -g @adobe/aio-cli`
安裝Adobe Developercli後，即可安裝mesh外掛程式。 要執行此操作，請運行此命令 `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
