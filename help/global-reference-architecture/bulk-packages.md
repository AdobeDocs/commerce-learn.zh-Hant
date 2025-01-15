---
title: 使用大量套件最佳化Adobe Commerce全球參考架構
description: 瞭解如何使用大量套件全域參考架構來設定Adobe Commerce，以有效進行程式碼管理和版本控制。
kt: 16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="由Adobe高級技術架構師Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰寫"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# 大量套件全域參考架構模式

本指南說明如何使用大量套件全域參考架構(GRA)模式來設定Adobe Commerce。

大量套件GRA模式涉及單一Git存放庫，以託管所有常見自訂。 這個單一Git存放庫會透過Composer公開為單一撰寫器套件，其中包含多個Adobe Commerce模組。

![顯示程式碼以大量封裝GRA模式儲存位置的圖表](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## 此模式的優缺點

優點：

- 透過共用程式碼存放庫重複使用程式碼
- 靈活地在不同執行個體上安裝不同歷史版本的GRA，以啟用分階段發行
- 彈性支援並維護GRA的多個主要版本
- 支援GRA的語意版本設定
- 簡單明瞭，開發人員不需要比一般單一商店開發模式更多的技能
- 不需要特殊的工具、複雜的基礎架構或特殊的分支策略
- 發行版本中的套件組合一律會一起開發和測試

缺點：

- 只能升級完整的GRA，包括其中所包含的所有套件。
- GRA大量套件不支援Adobe Commerce模組、語言套件和主題以外的撰寫器套件，因此無中繼套件、magento2元件套件、Composer外掛程式和修補程式

## 使用分割Git GRA模式設定Adobe Commerce

### 目錄結構

大量套件GRA會透過Composer存放庫安裝所有可重複使用的程式碼。 單一執行個體專屬的程式碼位於該執行個體的Git存放庫中。 執行個體特定的程式碼不會在Adobe Commerce的其他執行個體中重複使用。

使用大量套件GRA模式的完整Adobe Commerce安裝的最終目錄結構看起來像這樣：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

`app/code`、`app/i18n`和`app/design`目錄被有意省略，因為Composer不會評估這些目錄中的程式碼。 因此，在套件中宣告的相依性不會自動安裝。 大量套件GRA模式透過在`packages/`中安裝一些自訂程式碼並將該目錄視為撰寫器存放庫來解決此問題。 Composer將`packages/`內的封裝連結至`vendor/`。

### 準備Git存放庫

為共用GRA程式碼和第一個存放區建立兩個Git存放庫。 從具有以下檔案結構的GRA存放庫開始：

結果會是下列目錄結構：

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

此目錄結構僅提及GraOne和GraTwo模組的composer.json檔案和registration.php檔案。 實際上，這些模組中有更多檔案。

執行這些命令以起始Git存放庫：

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

composer.json檔案內容：

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

將composer.json套件的名稱空間變更為您自己的名稱空間。

src/registration.php檔案內容：

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

registration.php檔案會在Adobe Commerce模組內尋找其他registration.php檔案並執行它們。

使用<https://github.com/AntonEvers/gra-bulk-foundation>中的程式碼建立兩個範例模組。 Composer不會評估範例模組中的composer.json檔案。 他們習慣性地在那。 如果您決定將模組移動到其他位置，composer.json檔案會再次變為必要。

### 設定存放區存放庫

部署存放庫包含整個Adobe Commerce安裝，包括GRA程式碼。 建立部署存放庫：

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安裝Adobe Commerce。 使用撰寫器在部署存放庫中安裝GRA範例模組：

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

最後一個命令應該會產生以下輸出，以證明模組已安裝且運作正常：

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

您可以建立多個大量套件來組織程式碼。 例如，無法透過Composer取得的第三方程式碼第三方大量套件。 您傳統上安裝在`app/code`中的所有專案，現在應該都在大量套件的`src/`目錄中。 該規則的例外是僅用於單一執行個體的程式碼。 這些套件稱為本機套件。

### 安裝本機套件

部署儲存區域主控本機套裝程式。 它們不在GRA大量套件中。 本機套件的位置不是`app/code`，而是`packages/local`。 指示Composer將此目錄視為存放庫：

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

新增在<https://github.com/AntonEvers/module-example-local>上託管的範例模組：

```bash
mkdir -p packages/local
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

將本機模組提交至品牌存放庫：

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## 程式碼位置概觀

只有當協力廠商未透過Composer存放庫提供安裝時，您才能將協力廠商模組儲存在Foundation存放庫的`src/`目錄或專用的協力廠商大量套件中。

- **Adobe Commerce核心**：可透過repo.magento.com取得。
- **協力廠商模組**：可透過Marketplace或廠商自己的Composer存放庫取得。
- **協力廠商模組後援選項**：儲存在大量套件的`src/`中。
- **GRA Foundation程式碼**：儲存在Foundation大量封裝的`src/`中。
- **本機代碼**：儲存在部署存放庫的`packages/local`目錄中。

## 開發GRA模組

從來源安裝大量套件，以在大量套件目錄中啟用Git：

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

已使用Git簽出大量套件。 當您輸入`vendor/antonevers/gra-bulk-foundation`目錄時，您也會進入gra-bulk-foundation Git存放庫。 您可以在此目錄中建立、簽出及合併分支。

將Composer相依性新增到GRA大量套件根目錄下的composer.json檔案，這是Composer評估的大量套件中唯一一個檔案。

## 將協力廠商模組納入GRA大量套件

在GRA基礎根目錄下composer.json的required區段中新增協力廠商套件，以將它們新增至您的GRA。 如此一來，套件一律會透過Composer安裝在您的所有執行個體中。

## 傳遞您的程式碼

若要將程式碼傳送至主要分支，有2個路徑。 首先，本機模組，這些模組會合併至主要分支。 為這些模組執行撰寫器更新。 不允許開發人員更新其票證分支中的composer.lock以減少衝突。 僅更新中繼和生產分支中的composer.lock檔案，以降低衝突風險。

其次，GRA大量套件，這些套件合併到GRA大量存放庫的主要分支中。 然後，您可以將Git標籤新增到主要分支，為Composer套件建立版本。 需要在部署存放庫的composer.json中新版的GRA大量套件才能安裝。

## 分支策略

只要您在GRA大量存放庫中映象部署存放庫的分支策略，此GRA模式便會與所有分支策略搭配使用。 針對版本，在兩個存放庫中建立具有相同名稱的版本分支。 若為開發，請在兩個存放庫中建立票證分支。

在票證分支中，您幾乎永遠不必更新composer.lock檔案。 只需在開發環境中檢視正確的分支，即可使用Git找到存放區和GRA Foundation存放庫。 例外情況是當您更新GRA foundation composer.json檔案中的需求。 升級部署存放庫中的GRA基礎只有在建置版本或建置QA分支時才能完成。

## 程式碼範例

本文的程式碼範例可作為一組Git存放庫提供，用於測試概念證明。

- 範例生產存放區： <https://github.com/AntonEvers/gra-bulk-brand-x>
- GRA程式碼存放庫： <https://github.com/AntonEvers/gra-bulk-foundation>
- 範例本機模組： <https://github.com/AntonEvers/module-example-local>
