---
title: 全球參考架構Monorepo
description: 瞭解如何針對全球參考架構使用monorepo方法，以建立可擴充且可復原的商業體驗
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高級技術架構師Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰寫"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Intermediate, Advanced
source-git-commit: 421c6de9421d715127d0c892dc05ef22f77149cf
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---


# Monorepo全球參考架構模式

本指南說明如何使用Monorepo全域參考架構(GRA)模式設定Adobe Commerce。

Monorepo GRA模式涉及單一Git存放庫，以託管所有常見自訂。 這個單一Git存放庫會透過Composer公開為單獨的撰寫器套件。

![顯示程式碼在monorepo GRA模式中儲存位置的圖表](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 此模式的優缺點

優點：

- 適用於功能測試
- 透過共用程式碼存放庫重複使用程式碼
- 封裝安裝的完整彈性，每個GRA封裝都可以個別升級、降級或反向移植
- 完全支援語意版本設定
- 不需要特殊的工具、複雜的基礎架構或特殊的分支策略
- 支援Composer支援的所有套件型別
- 適用於短時環境（選填），但對於大量傳遞團隊則非常有用

缺點：

- 可能部署未以相同組態開發的套件組合，需要嚴格的測試程式
- monorepo GRA模式一開始可能很複雜。 指派協助團隊使用系統的潛在客戶

## 使用個別套件GRA模式設定Adobe Commerce

### 目錄結構

使用單獨套件GRA模式的完整Adobe Commerce安裝的最終目錄結構具有以下目錄結構：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

生產Git存放庫具有此目錄結構：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

不同之處在於，生產執行個體會從Composer安裝，其中monorepo在套件目錄中儲存每個套件的專屬副本。

### 準備生產存放庫

建立第一個Adobe Commerce例項的存放庫，代表Brand X的網站商店。

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安裝Adobe Commerce。 認可產生的`app/etc/config.php`和撰寫器檔案。 Composer會管理其他任何專案，因此Git中不應該有其他任何專案。

### 準備monorepo存放庫

monorepo存放庫的開頭步驟相同。

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

安裝Adobe Commerce與`bin/magento setup:install`，認可並推播。

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### 準備monorepo開發

對於monorepo開發，請對您的composer.json檔案進行以下變更：

1. 變更封裝的名稱和說明，如此一來就清楚知道此封裝是您的monorepo封裝。
1. 從composer.json刪除version屬性，因為版本是使用此存放庫的Git標籤來管理。
1. 以稍後建立的中繼套件取代require區段。
1. 將最低穩定性變更為開發。
1. 新增指向`packages/*/*`的路徑型別存放庫，以託管monorepo套件，包括它包含的每個套件的分支別名
1. 為專案本身新增分支別名

以下Git差異顯示全新Adobe Commerce安裝與上述變更之間的差異：

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### 使用中繼資料

從GitHub上的[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)下載範常式式碼，以取得此範例中使用的中繼套件和範例模組。

Composer中繼套件可將多個撰寫器套件繫結在單一套件下。 當需要中繼封裝時，其套裝的所有套件都會透過Composer自動安裝，並需要中繼封裝的區段。

在此範例中，有兩個中繼資料：

1. **antonevers/gra-meta-brand-x**：包含構成「品牌X」之所有內容的中繼套件
1. **antonevers/gra-meta-foundation**：包含任何品牌中一律安裝之所有內容的中繼套件

品牌中繼資料需要基礎中繼資料。 當需要品牌中繼資料時，基礎中繼資料也會自動需要。 請參閱中繼資料的兩個composer.json檔案，瞭解其關聯性：

antonevers/gra-meta-brand-x：

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

中繼封裝是整理屬於一起的程式碼的好方法。 使用中繼資料來定義地區、全球、品牌專屬或任何適合您的分組的套件群組。 如果您維護多個Adobe Commerce安裝，會以安全且多樣的方式定義預期套件所在的內容。

中繼資料存在於`packages`目錄內的monorepo中。 在那裡，`vendor`的目錄結構被映象：

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### 新增及開發模組

monorepo中的模組存在於`packages`目錄中。 這樣，Composer就能透過路徑型別存放庫找到它們。

從GitHub上的[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)下載範常式式碼，以取得此範例中使用的中繼套件和範例模組。

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

如有需要，您可以在`packages`目錄中擁有多個名稱空間。

開發會在封裝目錄中進行。 執行`composer update`之後，會在`vendor`目錄中建立指向`packages`目錄內封裝的Symlink。 如此一來，程式碼就會成為Adobe Commerce安裝的一部分。

執行`bin/magento module:enable --all`或只針對特定模組啟用新增的模組。

到現在為止，您應該已經安裝了正常運作的Adobe Commerce，且三個範例模組已安裝妥當並正常運作。 您可以透過執行下列命令來驗證模組是否已安裝及運作：

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### 實現自動套件建立

有多種選項可達成自動套件建立。 部分選項包括：

1. [私人封裝者](https://packagist.com/)
1. [簡化Monorepo Builder](https://github.com/symplify/monorepo-builder)
1. 建置您自己的解決方案

[Private Packagist](https://packagist.com/)會自動辨識Git monorepo中的套件，並透過Composer公開。 它與Adobe Commerce相容，速度快，維護工作量低，且容易出錯，因此本指南重點介紹私人套件組合選項。

說明如何設定私密封裝程式超出了本指南的範圍，請參閱[檔案](https://packagist.com/docs)。

當您設定組織同步且您的Git存放庫自動同步至私人套件組合時，就有可能將套件轉換為Monorepo。

首先，前往套件索引標籤，然後找到monorepo：

![私人封裝程式熒幕擷取畫面，在封裝畫面中顯示monorepo封裝](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

按一下monorepo套件，然後按一下詳細資訊畫面中的「編輯」，即可前往下列頁面：

![含monorepo封裝編輯頁面的Private Packagist熒幕擷取畫面](/help/assets/global-reference-architecture/packagist-packages-edit.png)

第一個輸入欄位底下有一個連結，顯示：建立多封裝存放庫。 按一下此連結。

![使用多重封裝組態的Private Packagist熒幕擷取畫面](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

定義monorepo中可找到composer套件的位置。 在此範例中，位置為`packages/**/composer.json`。 變更位置以限制或擴充Private Packagist搜尋要擷取之套件的位置。

封裝索引標籤會在儲存後顯示找到的所有封裝，而monorepo本身將不再顯示為Composer封裝：

![私人封裝程式熒幕擷取畫面，所有的monorepo封裝都顯示在封裝畫面中](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

在Composer中，會針對monorepo內的每個套件，以及在Git中的monorepo上建立的每個標籤或分支，建立版本。

## 將套件安裝至生產環境

依照私人套件管理員的指示，將私人套件管理員新增為撰寫器存放庫。 Private Packagist可以也應該作為您所有Composer存放庫和Git存放庫(包括packagist.org)的映象。 如此一來，認證就不必與開發人員共用，而您可以完全控制每個套件。 此範例不遵循此最佳實務，因為這會公開Adobe Commerce程式碼基底。

從GitHub下載[GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x)以檢視生產商店的範例。

在生產存放區中，沒有`packages`目錄，而且所有套件都是透過Composer安裝。 唯一需要的套件是：

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

然而，所有Adobe Commerce和整個GRA都是透過此中繼資料的要求進行安裝。

## 版本設定

monorepo中的所有套件都會收到與monorepo本身相同的版本。 將其視為發佈整個應用程式的新版本。 但在生產環境中，您可以從不同的monorepo版本安裝混合套件。

## 短暫環境

如果您使用短暫的環境或打算使用它們，monorepo是絕佳的選擇。 monorepo的每個版本和分支都包含所有Adobe Commerce、第三方和自訂模組檔案。 在每個分支中都完成安裝後，便可執行包括功能測試在內的各種測試。 搭配其他GRA設定（例如個別套件或大量套件GRA），您必須先建置運作中的Adobe Commerce環境，才能執行功能測試。 從DevOps的觀點來看，monorepo可大幅簡化程式。

## 程式碼範例

本文的程式碼範例已合併到一組Git存放庫中，您可以用來進行概念證明的遊戲。

- 範例monorepo存放庫： <https://github.com/AntonEvers/gra-monorepo>
- 範例生產存放區： <https://github.com/AntonEvers/gra-monorepo-brand-x>
