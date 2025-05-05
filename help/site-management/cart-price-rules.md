---
title: 建立購物車價格規則
description: 瞭解如何建立購物車價格規則，這些規則會根據一組條件在購物車中套用折扣。
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner
duration: 171
jira: KT-17148
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: d290ba1d9c8892b4322aeb19d3c65d9d8087a309
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# 建立購物車價格規則

購物車價格規則會根據一組條件，將折扣套用至購物車中的專案。 當滿足條件或客戶輸入有效優惠券代碼時，可自動套用折扣。 套用時，折扣會顯示在小計下的購物車中。 如有需要，可以透過變更其狀態和日期範圍，將購物車價格規則用於季節或促銷活動。

## 這部影片是給誰看的？

- 電子商務行銷人員
- 網站管理員

## 視訊內容

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## 定價顯示問題

有些獨特案例會要求每個明細專案顯示其所提供的折扣，但值可能並不完全相符。 原因是購物車價格規則折扣已套用至多個產品，但值未平均分成兩位小數。

>[!BEGINSHADEBOX]

購物車價格規則=套用至購物車中2項產品的10%折扣
價格規則生效的條件：購物車中的專案總數為2
動作會套用產品價格折扣的百分比，且折扣金額為10

有2個專案加入購物車，每個價值$19.95美元

若要取得折扣金額乘以產品價格0.1

19.95 x 0.1 = 1.995

這就是問題所在，我們有3位小數，而不是2位。 將這筆錢轉換為美元現在是個問題

>[!ENDSHADEBOX]

### 解決方案

回想一下唯一受此問題影響的人員，發現以美元折扣訂購的每件商品顯示最為合適。 為了確保正確計算整個訂單金額，決定舍入第一個專案，而其他專案捨棄第三個小數。 檢閱此情境：

>[!BEGINSHADEBOX]

如同上述購物車規則生效時的10%折扣
新增2項產品19.95版到購物車

每項產品可獲得$1.995的折扣
產品1 - 19.95 x 0.1 = 1.995
2 - 19.95 x 0.1 = 1.995

總金額為3.99的客戶可獲得折扣

在管理員中向商店所有者顯示條列專案時，
我們需要調整第一個專案，並將其四捨五入為2.000。第二個專案則會捨棄小數點後的第三個專案
產品1 = 2.00
產品2 = 1.99

這兩種產品加總後的總折扣符合提供給客戶的實際折扣。
>[!ENDSHADEBOX]

以下是熒幕擷圖，當訂單具有此情境時，其顯示在管理員中：

![管理員檢視顯示具有不同值的已排序專案](../assets/commerce-admin-cart-price-rule-values-different.png)

### 其他可能的解決方案，以及未使用它們的原因

>[!BEGINSHADEBOX]

如同上述購物車規則生效時的10%折扣
新增2項產品19.95版到購物車

每件產品可獲得$1.995的折扣，
不過，如果我們只是湊整一下，就會顯示太多折扣。

產品1 - 19.95 x 0.1 = 1.995
產品2 - 19.95 x 0.1 = 1.995

轉換為四捨五入所有專案
產品1新值為2.00
產品2新值為2.00

總金額為3.99，實際提供給客戶作為折扣。
但如果彙總，則會顯示已提供$4.00，這是不正確的。

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

類似的問題如果捨棄所有專案的第三個小數，則會顯示提供的折扣太少。

>[!BEGINSHADEBOX]

如同上述購物車規則生效時的10%折扣
新增2項產品19.95版到購物車

每項產品可獲得$1.995的折扣，但如果我們直接捨棄小數點後的第三個專案，就會發生下列情形：
產品1 - 19.95 x 0.1 = 1.995
產品2 - 19.95 x 0.1 = 1.995

轉換為拖放所有專案的第三個小數
產品1新值為1.99
產品2新值為1.99

總金額為3.99，實際提供給客戶作為折扣。
不過，如果我們捨棄小數點後三個，則會顯示$3.98已提供，而且不正確。

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## 其他資源

- [建立購物車價格規則 —  [!DNL Commerce] 銷售和促銷指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html?lang=zh-Hant)
- [優惠券代碼 —  [!DNL Commerce] 銷售與促銷指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html?lang=zh-Hant)
