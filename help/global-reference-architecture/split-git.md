---
title: 使用分割Git全域參考架構設定Adobe Commerce
description: 瞭解如何使用Split Git全域參考架構來設定Adobe Commerce，以有效管理程式碼並簡化部署。​URL
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe資深技術架構師Tony Evers撰寫" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰寫"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# 分割Git全域參考架構模式

本指南說明如何使用分割Git全域參考架構(GRA)模式來設定Adobe Commerce。

分割Git GRA模式涉及兩個用於開發的Git存放庫和每個Adobe Commerce執行個體的一個Git存放庫。 在這些範例中，假設每個例項代表唯一品牌。

![顯示程式碼以分割GRA模式儲存位置的圖表](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## 此模式的優缺點

優點：

- 透過共用程式碼存放庫重複使用程式碼
- 簡單的GRA模式，甚至適用於擁有有限撰寫器知識的團隊
- 除了Adobe Commerce模組、主題和語言套件之外，您也可以透過此模型安裝任何型別的Composer套件，包括composer-plugin、composer-metapackage、magento2-component和patches
- 可分階段發行，計畫於其維護視窗中發行至各區域
- 支援Git標籤用於管理目的，而非部署控制
- 保證在此確切的設定中開發和測試生產部署中的套件組合

缺點：

- 與其他GRA模式相比，沒有增加彈性
- 無法升級或降級每個執行個體的個別模組，請一律升級或降級GRA整體
- 在大多數情況下，大量封裝模式會更適合，因為它同樣簡單，但更傳統

## 使用分割Git GRA模式設定Adobe Commerce

### 目錄結構

分割Git GRA模式有兩種型別的存放庫：開發存放庫和安裝存放庫。 開發存放庫僅包含完整Adobe Commerce安裝的一部分。 安裝存放庫包含完整的Adobe Commerce安裝，並用於部署，但不用於開發。

使用分割Git GRA模式的完整Adobe Commerce安裝的最終目錄結構看起來像這樣：

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

`app/code`、`app/i18n`和`app/design`目錄被有意省略，因為Composer不會評估這些目錄中的程式碼。 因此，在套件中宣告的相依性不會自動安裝。 分割Git GRA模式透過在`packages/`中安裝所有自訂程式碼並將該目錄視為撰寫器存放庫來解決此問題。 Composer將`packages/`內的封裝連結至`vendor/`。

### 準備Git存放庫

建立3個Git存放庫：

1. Adobe Commerce執行個體
2. 未透過撰寫器安裝的協力廠商程式碼
3. 您的自訂，以模組、主題、語言包等形式呈現；您的GRA

在本指南中，以下名稱用於這些存放庫：

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

分割Git GRA模式中的所有存放庫會合併為一個。 若要Git允許合併多個存放庫，則所有三個存放庫都需要共用歷史記錄。 使用單一認可建立Git專案，並將其推送到所有遠端。

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

將暫存檔案`.gitkeep`推播到所有遠端，會建立具有相同認可雜湊的相同初始認可，進而建立共用歷史記錄。 在一個遠端中建立的每個變更都可以合併到其他變更中。

從這裡，存放庫發生了變化。 gra-split-brand-x存放庫包含品牌專屬的程式碼。 gra-split-3rdparty存放庫僅包含第三方程式碼。 gra-split-gra存放庫僅包含您的全域參考架構基礎，其中包含您所有自訂的程式碼。

在gra-split-brand-x存放庫中安裝Adobe Commerce。

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

在gra-split-3rdparty和gra-split-gra存放庫中建立初始認可。 最簡單的方法就是將這些存放庫簽出在不同的目錄中。

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

這兩個存放庫會儲存協力廠商套件和GRA套件。 每個Adobe Commerce例項都可能有專屬的程式碼。 建立一個位置，將這些本機套件儲存在gra-split-brand-x存放庫中。

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### 儲存不同程式碼型別的位置

Adobe Commerce是撰寫器應用程式。 首選的安裝方式一律透過Composer存放庫。 只有當模組供應商不透過Composer存放庫提供安裝時，您才能將第三方模組儲存在第三方存放庫中。 自訂程式碼的偏好位置位於GRA存放庫中。 當模組僅由一個特定執行個體使用時，它就會變成本機程式碼。

摘要：

- **Adobe Commerce**：儲存在Composer存放庫中。
- **協力廠商模組**：儲存在Composer存放庫中。
- **協力廠商模組後援選項**：儲存在gra-split-3rdparty Git存放庫中。
- **GRA foundation程式碼**：儲存在gra-split-gra Git存放庫中。
- **本機代碼**：儲存在gra-split-brand-x Git存放庫中。

### 將套件儲存裝置連線至Composer

Composer可以將packages目錄視為撰寫器存放庫。 通知Composer套件在套件目錄中的位置。

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer會在三個儲存目錄中尋找兩個層級深的composer.json檔案。 在三個程式碼儲存目錄內建立子目錄，就像它們出現在`vendor/`目錄中一樣。

執行個體：如果封裝通常安裝在`vendor/example-corp/module-example/`中，則將其儲存在`packages/3rdparty/example-corp/module-example/`中。 Composer會在您需要時將套件符號連結至`vendor/example-corp/module-example/`。

使用撰寫器套件名稱空間和名稱作為目錄結構。 例如：傳統上存在於`app/code/MyCorp/MyCustomization/`中的模組在composer.json中具有名稱`my-corp/module-my-customization`。 將此封裝儲存在`packages/gra/my-corp/module-my-customization`。

### 在執行個體存放庫中包含新套裝程式

將來自協力廠商和GRA的套件遠端合併到gra-split-brand-x存放庫中。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

結果會是下列目錄結構：

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

第三方和GRA Foundation存放庫中的變更會合併至品牌存放庫。 如此一來，第三方和GRA程式碼只會在一個地方進行維護。 透過Git合併將變更移至品牌。

Adobe Commerce不會自動辨識新模組。 執行撰寫器在合併後需要新增套件。 每次在合併後更新其中一個套件時，請執行撰寫器更新。

### 安裝範例模組

如需概念證明，請安裝範例模組以瞭解GRA模式的運作方式。

執行`composer install`和`bin/magento install`再繼續。

GitHub上有3個測試模組：

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### 安裝本機模組

將模組新增至本機程式碼集區相當簡單。 下載並解壓縮模組。 使用Composer時需要它。 使用`bin/magento`啟用它，並認可品牌存放庫中的檔案。

```bash
cd gra-split-brand-x
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

最後一個命令應該會產生以下輸出，以證明模組已安裝且運作正常：

```bash
Local module is installed successfully and working!
```

如果您看到上述輸出，則可以安全地將其提交至品牌存放庫。

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### 安裝及開發GRA基礎模組

將模組新增到GRA存放庫與安裝本機模組不同。 依預設，認可會新增至來源/主要（gra-split-brand-x存放庫）。 對GRA模組所做的變更應推送至gra-split-gra存放庫，並在之後合併至gra-split-brand-x存放庫。

### 建立開發環境

在一個位置建立包含所有程式碼集區的開發環境。 您可以透過symlink個別將程式碼推送至本機、GRA和協力廠商存放庫。 首先，在您的品牌、GRA和協力廠商存放庫目錄旁邊建立新的開發目錄。

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

結果會是下列目錄結構：

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

在gra-development目錄中執行`composer install`和`bin/magento install`。

現在可以直接從`packages/3rdparty`、`packages/gra`和`package/local`目錄認可變更。 Git會將變更提交至與目錄符號連結的Git存放庫。 在`packages/3rdparty`、`packages/gra`或`package/local`目錄中發出Git認可命令很重要。 請勿在專案根目錄執行Git認可。

### 安裝範例模組

在封裝目錄中安裝第三方和GRA範例模組。

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

最後一個命令應該會產生以下輸出，以證明模組已安裝且運作正常：

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

如果您看到上述輸出，則可以安全地將其提交至品牌存放庫。 執行`git remote -v`以確認您認可到正確的遠端。

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### 將程式碼傳遞至執行個體

將GRA和第三方存放庫合併至gra-split-brand-x存放庫，以將程式碼傳送至Adobe Commerce執行個體。 執行`composer require`、`bin/magento module:enable`並認可結果。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## 分支策略

如果您在第三方和GRA存放庫中映象存放區存放庫的分支策略，此GRA模式可與所有分支策略搭配使用。 針對版本，在所有三個存放庫中建立具有相同名稱的版本分支。 在發行準備期間將存放區存放庫上的發行分支合併在一起。

有時候，您的票證分支會要求變更本機程式碼和協力廠商程式碼或GRA程式碼。 在這種情況下，需要在所有相關存放庫中建立票證分支。

切勿將第三方和GRA認可合併至票證分支內的品牌存放庫。 反之，請檢視開發環境中每個程式碼集區的正確分支。 只有在構成版本或構成QA分支時，才能合併至品牌存放庫。

## 程式碼範例

本文的程式碼範例可作為一組Git存放庫提供，用於測試概念證明。

- 範例生產存放區： <https://github.com/AntonEvers/gra-split-brand-x>
- 協力廠商程式碼存放庫： <https://github.com/AntonEvers/gra-split-3rdparty>
- GRA程式碼存放庫： <https://github.com/AntonEvers/gra-split-gra>
- 範例本機模組： <https://github.com/AntonEvers/module-example-local>
- GRA模組範例： <https://github.com/AntonEvers/module-example-gra>
- 範例第三方模組： <https://github.com/AntonEvers/module-example-3rdparty>
