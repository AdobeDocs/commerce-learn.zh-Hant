---
title: 使用Fastly拒絕存取整個網站
description: 使用Fastly Edge ACL和自訂VCL限制Adobe Commerce網站存取
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# 使用Fastly拒絕整個網站的存取

瞭解如何使用Fastly Edge ACL和自訂VCL片段來限制對您的Adobe Commerce Cloud網站的存取。 此逐步指南僅允許特定IP位址，確保您的開發網站維持私密狀態，協助您保護啟動前環境。

## 您將瞭解的內容

使用Fastly Edge ACL和自訂VCL限制Adobe Commerce網站存取 | 安全的啟動前環境

## 這部影片是給誰看的？

* DevOps工程師
* Adobe Commerce開發人員
* 網站可靠性工程師

>[!VIDEO](https://video.tv.adobe.com/v/3464790/?learn=on&enablevpops&captions=chi_hant)

## 程式碼範例

這是所用VCL的範例

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## 相關檔案

* [正在偵測惡意IP位址](https://experienceleague.adobe.com/zh-hant/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [允許要求的自訂VCL](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [封鎖要求的自訂VCL](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)