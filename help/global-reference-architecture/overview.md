---
title: 使用Adobe Commerce最佳化程式碼重複使用
description: 瞭解如何使用全域參考架構模式最佳化Adobe Commerce中的程式碼重複使用，以提升多個執行個體的效能和合規性。
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高級技術架構師Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰寫"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# 全球參考架構實作技術

有數種方式可透過Adobe Commerce最佳化程式碼重複使用。 這四種實作技術各有其優點。 本文的範例依序由簡單到複雜。 選擇最適合您專案和未來藍圖的策略。 從一種策略遷移到另一種策略可能很耗時。

## 何時該使用全球參考架構

視您擁有的執行個體數量而定，全域參考架構可能相當實用。 執行個體是使用自身資料庫獨立安裝的Adobe Commerce。 計算生產資料庫的數量，以瞭解您擁有多少執行個體。 如果您維護多個執行個體，或者如果您預見未來會發生這種情況，則可從全域參考架構中獲益。 執行個體共用的功能越多，全域參考架構增加的價值就越高。

在以上任何一種情況下，建議您使用Adobe Commerce的多個例項來進行探索。

1. **不同的商店擁有者**：如果您維護多個商店擁有者的程式碼，而每個擁有各自的不同商店，則可能需要個別的執行個體才能有效維護其個別需求。
2. **遵守國家法規**：某些法規規定客戶資料必須儲存在特定區域。 在這種情況下，必須個別執行個體，以確保符合這些法規。
3. **跨地理區域的作業差異**：在多個區域作業可能代表不同的維護排程和需求。 使用個別例項可讓您靈活有效地管理這些變化。
4. **高強度Flash銷售**：進行大規模快閃磁碟銷售的商店通常需要最佳化的伺服器效能。 由個別執行個體提供的專用基礎建設，可確保在這類高需求期間提供最佳效能。
5. **品牌或國家/地區之間的重大差異**：當品牌或國家/地區之間的差異很大時，使用單一執行個體會產生僅用於某些品牌或國家/地區的程式碼。 個別例項可消除不需要程式碼的品牌和國家/地區不必要的程式碼，進而提升效能和穩定性。

## 全球參考架構模式

在「無GRA圖樣」旁邊，有四種GRA圖樣的樣式。

![5個GRA模式的圖示：無GRA、分割、大量、單獨和單向。](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### 無GRA模式

![描述「無GRA」的圖示](/help/assets/global-reference-architecture/no-gra.png){align="center"}

未使用GRA模式時，每個Adobe Commerce例項都是唯一的應用程式。 除了手動將程式碼從一個執行個體移動到另一個執行個體外，沒有程式碼重複使用功能。 這些復本總是有差異。 確保每個執行個體具有相同的變更但仍如預期運作所需的工作量，可能會變得讓人不知所措。 此情境中，三個執行個體需要三倍於一個執行個體的維護工作。

![圖表顯示3個商店，每個商店都是前者的復本，所有3個復本都有獨特的開發。](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### 分割Git GRA模式

![描述「分割」GRA模式的圖示](/help/assets/global-reference-architecture/split-git.png){align="center"}

此模式包含用於開發的Git存放庫和每個執行個體的一個Git存放庫。 執行個體中的每個檔案都會在其中一個開發存放庫中維護。 它們一起組成了整個GRA。 每一行程式碼都只存在於單一開發存放庫中，並使用編排技術安裝在執行個體中，導致程式碼重複使用。

![顯示程式碼以分割GRA模式儲存位置的圖表](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### 大量封裝GRA模式

![代表「大量」GRA模式的圖示](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Adobe Commerce核心和協力廠商模組會透過Composer存放庫直接安裝。 Git存放庫可用作Composer存放庫。 在此模式中，整個GRA共用程式碼基底會託管在單一或數個Git存放庫中，並透過Composer安裝。 主要特色是多個模組、語言套件或主題託管於單一撰寫器套件，可簡化開發。

![顯示程式碼以大量封裝GRA模式儲存位置的圖表](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### 獨立的套件GRA模式

![代表「個別封裝」GRA模式的圖示](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

每個Adobe Commerce模組、語言套件或主題都會安裝為個別的撰寫器套件。 每個自訂都有自己的Git存放庫。 這表示執行個體的構成具有極致的彈性，並且具有可靠的撰寫器相依性管理。 為了最佳化效能，所有套件都映象到單一私人撰寫器存放庫中。

![顯示程式碼儲存在不同套件GRA模式中的位置的圖表](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### monorepo GRA模式

![代表「monorepo」 GRA模式的圖示](/help/assets/global-reference-architecture/monorepo.png){align="center"}

所有開發都會在單一程式碼存放庫中進行。 自動化會產生新版本的套件，並將其發佈到撰寫器存放庫。 此模式結合了大量套件方法的低開發開銷與個別套件方法的靈活性。 monorepo模式也非常適合執行自動化功能測試。

![顯示程式碼在monorepo GRA模式中儲存位置的圖表](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 選擇GRA圖樣

GRA模式的選擇是透過評估專案複雜性、對彈性的需求和開發團隊的適應能力來做出的。

Adobe Commerce經驗不足的團隊最好從簡單開始。 但是，如果專案由於其特性而需要更複雜的GRA模式，請勿妥協。

與每個模式相關的常見專案特性：

1. **沒有GRA模式**：沒有要擴充的計畫的Adobe Commerce單一執行個體。 多個Adobe Commerce例項，且彼此之間幾乎沒有共同之處。

2. **分割Git GRA模式**：想要避免Composer進行自訂的團隊，在大多數情況下，大量套件模式是分割Git的偏好模式。

3. **大量套件GRA模式**：具有高互依性的自訂程式碼基底。 執行個體都有非常相似的自訂套件組合。 不經常升級或降級個別套件。 團隊在程式碼管理方面經驗很少，而且需要簡單化。

4. **獨立的套件GRA模式**：需要彈性的發行範圍管理。 預計未來5年內將有50個或更少的自訂套件。 可能是通用程式碼的全球和區域層。 沒有移轉至Monorepo模式的計畫。 該團隊在技術上非常熟練，並且嚴格遵守流程。

5. **Monorepo GRA模式**：獨立封裝GRA模式的所有特性。 未來5年預計將有50個以上的套件。 需要廣泛的自動化測試。 支援短暫環境。 企業規模的複雜程式碼基底，需要在不以相同速率擴充維護成本的情況下進行擴充。

### 錯誤選擇的成本

從一種模式移轉至另一種模式是可行的。 下圖顯示從一種模式移至另一種模式的影響程度。 綠色線條顯示影響較低，黃色和琥珀色線條顯示複雜移轉的程度適中。

![圖表顯示所有4個GRA圖樣之間的彩色箭頭，表示從一種圖樣移至另一種圖樣的困難程度。](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
