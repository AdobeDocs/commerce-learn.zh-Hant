---
title: 偵測IP位址
description: 瞭解如何偵測Adobe Commerce雲端環境的IP位址，以強化安全性並簡化伺服器通訊
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
source-git-commit: a14a878217a145ecee0b29247ec7ccb224edd883
workflow-type: tm+mt
source-wordcount: '1106'
ht-degree: 0%

---


# 瞭解如何偵測Commerce Cloud專案中所有環境型別的IP位址

瞭解如何在Adobe Commerce Cloud專案中偵測不同環境的IP位址。 透過使用一系列命令，包括Adobe Commerce CLI、sed、xargs、dig、grep和sort -u，使用者可以識別用於開發、測試和生產環境的IP位址。

## 這部影片是給誰看的？

* 開發人員：瞭解如何收集Adobe Commerce Cloud專案的IP位址。
* 需要限制存取協力廠商或後端系統的DevOps和安全團隊

## 視訊內容 {#video-content}

* 瞭解如何在Adobe Commerce Cloud中找出任何環境的IP位址。

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## 取得IP位址的命令

請注意，您需要使用專案ID和環境名稱，而非預留位置資訊。  可能也需要變更`{1..3}`以符合Adobe Commerce Cloud叢集中的節點數，但3個最常見。

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

magento-cloud CLI工具旨在協助開發人員和系統管理員有效管理Adobe Commerce Cloud專案和環境。 它擴充了Cloud Console的功能，讓使用者能夠執行日常工作並在本機執行自動化。 主要功能包括管理整合環境、簽出和合併環境、列出變數以及使用SSH連線到遠端環境。 此工具允許直接從本機工作站執行命令，藉此簡化工作流程，進而增強整體開發和部署程式。

在範常式式碼的這個初始區段中，`magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1`它正在要求環境的URL。 傳回的值看起來像這樣`http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`。 偶爾會看起來更像此`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`。  第一個指令相當簡單，現在可以移至下一個指令。

如需詳細資訊，請閱讀[雲端CLI總覽](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## 使用`sed`進行搜尋和取代

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

UNIX®Linux®中的命令`sed`代表資料流編輯器。 它用於在輸入資料流（檔案或來自管線的輸入）上執行基本文字轉換。 常見的用途包括搜尋、尋找和取代、插入和刪除文字。 命令`sed`會逐行處理文字，並套用指定的作業，使其成為強大的文字操作與指令碼工具。

如前所述，通常從`magento-cloud` cli傳回兩種型別的URL。 中間有一個包含`.com.c.c`的變數。 此變體是需要處理的變體。 如果偵測到此結構，則需要移除從URL開頭到`.com.c.c`的所有專案。  剩下的只是URL的最後一部分。 範例URL看起來像`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`。  偵測到此模式時，目標是在`.c.`之後保留所有內容。  在此提供的範常式式碼中，`sed 's/.\.c\.(.)/\1/'`是用來抓取此部分，並忽略原始傳回值的其餘部分。 URL的其餘部分類似於`abcikdxbg789.ent.magento.cloud/`。\
在`sed`中有兩個正在執行的命令。 它們以分號分隔。 我的`sed`命令`;s/.$//'`的第二個部分是移除任何尾端斜線（如果存在），以清除該URL看起來像`abcikdxbg789.ent.magento.cloud`。  此時，URL已清除，並準備好執行下一個命令。

## 使用dig的Xargs

```bash
xargs -I% dig +short {1..3}."%"
```

UNIX®Linux®中的`xargs`命令用於從標準輸入建立和執行命令列。 它會從管路或檔案中取得輸入，並將其轉換為另一個命令的引數。 它對於處理超過殼層限制的大量引數特別有用。 命令`xargs`可用來執行如移動、複製或刪除檔案等作業。 它可在單一執行中將多個引數傳遞至命令，以實現高效的批次處理。

`dig`命令（Domain Information Groper的簡稱）是用來查詢DNS （網域名稱系統）伺服器的網路管理工具。 它有助於擷取有關DNS記錄的資訊，例如A、AAAA、MX和CNAME記錄。 命令`dig`通常用於疑難排解DNS問題、驗證DNS設定，以及收集有關網域名稱及其相關IP位址的詳細資訊。 透過使用各種選項和旗標，使用者可以自訂輸出以顯示特定的詳細資訊或簡明的摘要。

搭配`dig`使用`xargs`會使其變得複雜，但這是必要的。 目標是要取用清理過的URL並加以儲存。  將URL儲存為變數後，就會將它插入`dig`命令中。

已建立命令`dig`來收集DNS資訊。 若要減少傳回的資料量，請使用引數`+short`。 使用`dig`結合`+short`後，會傳回IP位址，有時還會傳回字串。

在該命令部分，`xargs`將儲存該URL `abcikdxbg789.ent.magento.cloud`並將其插入下一個命令`dig`中。 儲存URL與反複專案結合的技術，可讓您更輕鬆地搭配`dig`命令使用。 請記住，我的程式碼範例是實現目標的方式之一，您可以隨時修改內容以符合您的需求和期望。

此時，URL已準備就緒。 接下來，讓我們看看如何檢查叢集中的每個伺服器。 若為Adobe Commerce Cloud，叢集中的每個伺服器都會有一個數字。 伺服器識別碼是剛清除並準備就緒可供使用的URL的前置詞。 使用`{1..3}`可快速且輕鬆地取出伺服器。 使用通知`dig`命令執行3次的`{1..3}`。 以下表示如果您要即時觀看執行，會發生什麼情況。

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

為了更好的說明目的，`dig`使用的這個範例URL會類似：

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

如果`{1..3}`修改為`{1..6}`，則會如下所示：

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## 命令`grep`

有時候，會從`dig`傳回字串作為結果的一部分。 針對此目的，目標只是IP位址，而且由具有句號的數字組成。 若要確保最終輸出只包含數字，可以調整指令。 完成時，最終語法為` grep '^\d'`。  新增`'^\d'`後，命令`grep`只會保留數字，而忽略其他任何專案。

## 命令`sort`

藉由使用`sort -u`，它會從IP清單移除任何重複專案。 只有在檢視開發層級時才會發生重複專案。

這些較低層級環境是多租使用者，並與許多其他專案共用基礎伺服器。 開發環境是單一伺服器，而非叢集。 因此，當dig指令在每次疊代上循環時，會多次傳回相同的IP。 因此，使用命令`sort -u`會移除所有重複的IP，而只保留唯一的IP位址。



## 相關檔案

* [地區IP位址](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses|https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
