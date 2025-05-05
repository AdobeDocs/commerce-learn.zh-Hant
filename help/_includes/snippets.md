---
title: 版本橫幅
description: 重複使用的視覺元素，以記下套用至特定版本的功能或頁面
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# 版本橫幅

## 僅供檢視的功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce功能" src="../assets/adobe-logo.svg" width="20" height="20" /> 只有Adobe Commerce才有專屬功能（<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=zh-Hant#product-editions">深入瞭解</a>）</td></tr>
</table>

## 僅限B2B功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce功能" src="../assets/b2b.svg" width="20" height="20" /> 只有<a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html?lang=zh-Hant">B2B才能使用Adobe Commerce</a>的專屬功能</td></tr>
</table>

## 400個問題 {#avoid-400-error}

>[!CAUTION]
>
>執行API呼叫時，請確定已使用某種searchCriteria。 您也可以考慮使用分頁。 如果Adobe Commerce的結果太大，可能會符合Adobe Developer App Builder容量，並導致檔案意外結束。 這會導致400錯誤的回應結果。\
> 例如，假設需要向Adobe Commerce請求所有目前產品。 產生的URL類似於`{{base_url}}rest/V1/products?searchCriteria=all`。 根據傳回的目錄大小，json可能太大，App Builder無法取用。 改為使用分頁功能，並提出一些要求以避免`Response is not valid 'message/http'.`
