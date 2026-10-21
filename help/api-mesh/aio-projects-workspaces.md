---
title: 使用專案和工作環境
description: 瞭解如何使用Adobe Developer Console搭配API Mesh的專案和工作區使用，包括新增網格、透過CLI登入及切換組織。
jira: KT-11803
doc-type: Tutorial
duration: 572
last-substantial-update: 2026-08-03T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: ab51f68c-5d28-495b-8472-27b60c4aa8c1
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%
---
# 使用專案和工作環境

本教學課程討論如何建立專案和工作區，以包含您的網格。 此工作主要在[Adobe Developer主控台](https://developer.adobe.com/console){target="_blank"} UI中進行。

## 這部影片是給誰看的？

* 有權存取Adobe Developer Console帳戶且想要建立專案和工作區的開發人員。

## 視訊內容

* 在Adobe Developer Console中使用專案和工作區
* 在Adobe Developer Console中將API網格新增至工作區
* 在CLI中登入Adobe Developer主控台
* 在CLI中檢視選取的專案和工作區
* 在CLI中變更選取的組織、專案或工作區
* 測試簡單的API Mesh命令

>[!VIDEO](https://video.tv.adobe.com/v/3430774?captions=chi_hant&learn=on)

## 瞭解Adobe Developer Console

在Adobe Developer Console中，下列階層說明您的API在您組織中的組織方式： `Organization > Project > Workspace > [API]`。 若要深入瞭解Adobe App Builder、登入主控台及基本的疑難排解，請造訪[建立您的第一個App Builder應用程式](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app){target="_blank"}。

## Adobe Developer Console專案簡介

Adobe Developer Console中的所有開發工作都會作為專案的一部分執行。 專案可包含一或多個產品，以及API、事件、執行階段和外掛程式的組合。 如需Adobe Developer主控台中專案的詳細資訊，請造訪[專案](https://developer.adobe.com/developer-console/docs/guides/projects/){target="_blank"}。

如需在API Mesh內容中使用專案和工作區的詳細資訊，請參閱[修改專案和工作區](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}。

{{$include /help/_includes/api-mesh-related-links.md}}
