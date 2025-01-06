---
title: 個別套件全球參考架構
description: 使用獨立的套件GRA最佳化Adobe Commerce。 瞭解彈性的版本化套件管理的設定、好處和最佳實務。
kt: 16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
source-git-commit: 916586d3b71b8b74baa04af2ab39abc86ec94f5b
workflow-type: tm+mt
source-wordcount: '2088'
ht-degree: 0%

---


# 獨立套件全域參考架構模式

本指南說明如何使用個別套件全域參考架構(GRA)模式來設定Adobe Commerce。

獨立套件GRA模式涉及每個共同套件的一個Git存放庫和每個Adobe Commerce執行個體的Git存放庫。 透過具有私人撰寫器存放庫的Composer公開常見套件。

此全域參考架構模式完全以Composer為基礎，旨在從所有Composer功能中獲得最大利益。

![顯示程式碼儲存在獨立封裝GRA模式中的位置的圖表](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## 此模式的優缺點

優點：

- 透過共用程式碼存放庫重複使用程式碼
- 封裝安裝的完整彈性，每個GRA封裝都可以個別升級、降級或反向移植
- 完全支援語意版本設定
- 不需要特殊的工具、複雜的基礎架構或特殊的分支策略
- 支援Composer支援的所有套件型別

缺點：

- 在這個GRA模式中的開發在開始時會稍微困難一些，會有小段學習曲線
- 可能部署未以相同組態開發的套件組合，需要嚴格的測試程式

## 使用個別套件GRA模式設定Adobe Commerce

### 目錄結構

使用單獨套件GRA模式的完整Adobe Commerce安裝的最終目錄結構如下所示：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

有意省略`app/code`、`app/i18n`和`app/design`目錄。 獨立套件GRA會安裝Composer的每個單一套件。 即使套件僅安裝在單一Adobe Commerce執行個體中。

### 準備存放區存放庫

建立第一個Adobe Commerce例項的存放庫，代表Brand X的網站商店。

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安裝Adobe Commerce。 認可產生的`app/etc/config.php`。

### 建立套裝程式儲存區域

此全域參考架構模式中的每個套件都有自己的Git存放庫。 以下是包含Adobe Commerce模組的範例套件，這些模組代表GRA模組、第三方模組及本機模組。

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

使用範例建立您自己的套件。

### 建立中繼套件存放庫

中繼套件控制這個GRA模式中GRA通用程式碼基底的範圍。 它們定義基礎的內容：永遠一起安裝的套件版本組合。 範例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

上述程式碼片段是中繼資料的composer.json。 因為中繼資料只包含composer.json檔案，不含其他程式碼。 上述程式碼也是完整的中繼功能。 將其託管在Git存放庫中，您便擁有可安裝的中繼撰寫器存放庫。 它需要一個範例GRA模組和協力廠商模組，以及Adobe Commerce核心。 這同樣需要gra-component-foundation，將於下一章中說明。

中繼封裝是一種在不建立封裝之間的相依性的情況下將封裝套裝成束的方法。 因此，即使套件之間沒有技術相依性，透過中繼套件，您可以使它們一起安裝。 如果您在專案中需要此中繼封裝，則會安裝該中繼封裝所需的任何封裝或中繼封裝。 因此，如果您建立空白撰寫器專案，而您只需要此套件，則Composer會安裝Adobe Commerce以及GRA和協力廠商模組。

如此一來，您便可確保每個存放區都包含相同的基礎套件集。

您可以用類似方式定義定義存放區x的中繼包。它需要基礎中繼資料，需要完整的GRA基礎以及本機模組：

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Brand-X中繼是選用專案。 您也可以略過品牌中繼資料，並直接在存放區撰寫器專案中要求這些相依性。 建立本機模組的中繼資料的優點在於，您在存放區Git存放庫中（僅在套裝資料存放庫中）沒有任何功能分支和功能提取請求。 這是一項安全措施。 此外，您可以選擇在套裝程式存放庫上套用語意版本設定，並在您的主要專案上使用不同的Git標籤，例如追蹤已命名的版本。 由您決定。

### 廠商目錄之外的GRA foundation檔案

有時您需要將檔案儲存在廠商目錄之外。 例如`.gitignore`，位於`dev/`目錄或網域驗證檔案中的檔案。 magento2-component封裝型別就是為此目的而設計。 檢視<https://github.com/AntonEvers/gra-component-foundation>。

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

此套裝軟體具有magento2-component型別，且包含src目錄，用來託管已複製到Adobe Commerce根目錄的檔案。 此檔案中的對應會將`/src/gitignore`複製到主要撰寫器專案中的`/.gitignore`。

如此一來，您就可以將廠商目錄以外的檔案也做為GRA基礎的一部分。

### 開發GRA基礎模組

開發會在廠商目錄中進行。 請要求Composer從來源安裝您的Foundation套件。 如此一來，會從Git簽出套件，而不是從下載的封存安裝它們。

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

透過此命令，Antonevers名稱空間中的套件已使用Git簽出。 當您輸入vendor/antonevers/module-gra目錄時，也會輸入module-gra Git存放庫。 您現在可以直接從廠商目錄建立、簽出和合併分支，並透過這種方式進行開發。

### 將協力廠商模組納入GRA基礎

將協力廠商套件新增至GRA中繼套件。 如果無法從撰寫器存放庫安裝協力廠商程式碼，請為其建立套件。 建立Git存放庫、新增套件內容（應用程式/程式碼/廠商/套件中的所有內容），並確保在存放庫的根目錄下有有效的composer.json檔案。 您現在可以透過Composer安裝此套件。

## 設定私人撰寫器存放庫

全球參考架構中不強制使用私人存放庫。 它可加快部署和安裝速度、減少composer.json中的存放庫設定，並提升安全性。 其他Composer存放庫和Adobe Commerce Marketplace的認證會儲存在您的私人存放庫中。 程式碼或開發人員的電腦上不再隨附敏感認證。

此外，有些私人存放庫會提供額外功能，例如當其中一個存放庫在其其中一個相依性中包含安全性弱點時，提供電子郵件通知。

當您在composer.json中有多個VCS存放庫時，就會發生速度緩慢問題。 進行升級時，需要讀取每個Composer存放庫，而且對於50個套件有50個存放庫，其製造費用至少是單一Composer存放庫的50倍。

![圖表顯示當缺少撰寫器存放庫時緩慢的發生位置](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

以私人Composer存放庫的形式包含Composer映象。 映象包含來自其他Composer存放庫以及所有Git裝載之套裝軟體的副本。 有了私人Composer存放庫，您還能取得更精細的存取控制。

透過Git同步處理，私用Composer存放庫會自動偵測您Git存放庫中的新套裝軟體，以及現有套裝軟體的新版本。

您可以使用Satis來託管您自己的私人存放庫： <https://composer.github.io/satis/>。 檢視位於<https://antonevers.github.io/gra-composer-repository/>的公用存放庫範例。 此存放庫會作為程式碼範例中的撰寫器存放庫。 若要將Satis存放庫設為私用，必須採取其他措施。

您可以設定並忘記以下解決方案：私人封裝程式<https://packagist.com/>，是由撰寫Composer或JFrog Artifactory <https://jfrog.com/artifactory/>的同一人所製作。

## 傳遞代碼

使用中繼套件，有3個步驟可傳送程式碼。

1. 將變更合併到封裝中，並將變更的封裝版本化。
2. （選擇性，只有在新增新套件時才會出現）需要中繼套件中的新套件和版本中繼套件。
3. （選用，僅在新增新套件時）需要在Adobe Commerce中新的中繼資料並部署。

使用套件版本控制部署範圍。 建立穩定版本的套件表示此套件已準備好進行生產部署。

若要建置新版本，請在包含完整商店安裝的主要Composer專案中執行composer更新。 已安裝所有最新版本的套件。

## 版本設定

個別套件GRA中的版本設定是Git中標籤模組的同義詞。 Git標籤會建立Composer所安裝的套件編號版本。
正確的版本設定方法可讓您的套件自動流動，同時保持安全。

兩個範例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

此範例顯示相依性的嚴格定義。 精確版本中需要3個套件。 在您的安裝中使用這個中繼資料更新Composer不會起任何作用。 即使有較新版本可用，此版本也會一律將這3個套件安裝在這些精確版本中。

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

此範例顯示相依性的鬆散定義。 透過`~1.0`，如果任何版本的這些套件大於或等於`1.0.0`且小於`2.0.0`，則可以安裝這些套件，且具有最高可用版本的偏好設定。 您可以在<https://getcomposer.org/doc/articles/versions.md>進一步瞭解如何定義版本相依性：

> ~運運算元最好以範例說明： `~1.2`等於`>=1.2 <2.0.0`，而`~1.2.3`等於`>=1.2.3 <1.3.0`。

一旦您發行提及的任何套件的新版本，它就會自動隨Composer更新安裝。

套用語意版本設定。 您可以在<https://semver.org/>學習有關語意版本設定的一切。 尤其是，常見問題集是必須閱讀的。 透過語意版本設定，「1.0.0」中的數字稱為MAJOR.MINOR.PATCH。 封裝的次要和修補程式發行版本應該可以安全引進，而不會中斷應用程式。
您可以自動包含修補程式，並手動選擇次要升級。 請注意，若要這麼做，請手動挑選每項次要變更，以節省額外的負荷：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

當然，唯有您一律一致地套用語意版本化，這一切才有用。 不僅在中繼套件中，您一般套件的需求也應該寬鬆地定義相依性。 如果您的系統中具有一個嚴格相依性，則該套件受限於嚴格定義。

鍵入composer dependents \&lt;package name\>以尋找這些嚴格的相依性。 如需詳細資訊，請參閱<https://getcomposer.org/doc/03-cli.md#depends-why>。

## 分支策略

您可以使用各種分支策略來支援此全域參考策略模式，前提是主要分支是您設定套件版本的唯一分支。 如果您跨多個分支進行版本，則會帶來在版本之間隨機遺失功能的風險。 只會在主要分支上建立穩定版本。

只會在您的套裝程式存放庫中建立功能分支。 不在您的存放區安裝存放庫上。 只要使用Composer，就能繼續為您的商店引進任何變更。 避免Git需要在部署存放庫中合併。

分支策略和存放庫中具有常見的分支型別，這些分支應存在於以下位置：

**功能分支**：位於您的封裝存放庫中，絕無其他。

**發行分支**：是在任何存放庫中所建立：套件、中繼套件、存放區安裝存放庫。 當您計畫發行時，在套件的發行分支中群組變更後，再進行版本設定。 假設您正在準備程式碼名稱為「Unicorn」的發行版本。 您可以在包含變更的套件中建立「發行版本獨角獸」分支。 將其中任何內容合併，然後在您的中繼資料中需要「dev-release-unicorn as 1.4.0」。 深入瞭解別名，瞭解發生的情況： <https://getcomposer.org/doc/articles/aliases.md>。

**QA/Dev分支**：類似於發行分支。

**主要分支**：必須存在於每個存放庫中，且應該永遠是代表生產或生產就緒狀態的分支。 主要分支是您標籤發行版本程式碼的位置。
請務必選擇維護開銷很少的分支策略。 例如，在Hotfix版本之後將主要分支合併回QA、UAT、Release或Dev分支，是一項經常性維護任務。 套裝程式越多，儲存區域就越多，重複性的管理費用任務也越多。

使用mixu/gr之類的工具，在批次中的多個Git存放庫上執行例行作業： <https://github.com/mixu/gr>

## 命名慣例

使用獨立套件GRA模式時，如果基礎中繼套件需要套件，則這些套件是GRA基礎的一部分。 從中繼封裝新增或移除封裝，以將它們移入和移出基礎。

中繼套件可讓套件的安裝範圍更具有彈性。 套件名稱中不包含與封裝預期用途相關的任何字詞特別重要。 當您決定將該封裝移出GRA基礎時，名稱antonevers/module-gra-store-locator可能會變得混亂。 避免範圍(GRA、foundation、local)。 避免地區（歐洲、中東和非洲、西班牙、全球）。 絕對會避免使用建置套件的存放區名稱。 選擇僅與封裝中所新增的功能相關的名稱。 這樣一來，您就可以在任何地方重複使用它們，在無法預見的未來情況下也是如此。 名稱antonevers/module-store-locator會很棒。

請確定相關套件一起顯示在概述中。 從一般到特定的建置名稱。 因此，Antonevers/module-b2b-tax-exempt而非antonevers/tax-exempt-module-b2b。

## 程式碼範例

此部落格的程式碼範例已合併到一組Git存放庫中，可供您用來進行概念證明的遊戲。

- 範例生產存放區： <https://github.com/AntonEvers/gra-separate-brand-x>
- 基礎模組範例： <https://github.com/AntonEvers/module-example-gra>
- 範例第三方模組： <https://github.com/AntonEvers/module-example-3rdparty>
- 範例本機模組： <https://github.com/AntonEvers/module-example-local>
- 基礎中繼資料範例： <https://github.com/AntonEvers/gra-meta-foundation>
- 本機中繼資料範例（選擇性）： <https://github.com/AntonEvers/gra-meta-brand-x>
- 範例撰寫器存放庫： <https://github.com/AntonEvers/gra-composer-repository>