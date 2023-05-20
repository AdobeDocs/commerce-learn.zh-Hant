---
title: 版本橫幅
description: 重新使用可視元素來注釋應用於特定版本的特徵或頁面
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 版本橫幅

## 僅EE功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce特徵" src="../assets/adobe-logo.svg" width="20" height="20" /> 獨家功能僅在Adobe Commerce(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">瞭解更多資訊</a>)</td></tr>
</table>

## 僅B2B功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce特徵" src="../assets/b2b.svg" width="20" height="20" /> 獨佔功能僅可用於 <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Adobe CommerceB2B</a></td></tr>
</table>

## 400期 {#avoid-400-error}

>[!CAUTION]
>
>執行API調用時，請確保使用某種searchCriteria。 您也可以考慮使用分頁。 如果Adobe Commerce的結果太大，可能會滿足Adobe Developer應用生成器容量，並導致檔案意外結束。 結果是錯誤的響應結果，因為400錯誤。\
> 例如，假設需要從Adobe Commerce請求所有當前產品。 生成的URL與 `{{base_url}}rest/V1/products?searchCriteria=all`。 根據返回的目錄大小，Json可能太大，App Builder無法使用。 而是使用分頁並發出一些請求以避免 `Response is not valid 'message/http'.`
