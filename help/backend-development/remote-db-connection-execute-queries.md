---
title: 連線並執行對資料庫的查詢
description: 瞭解連線至Adobe Commerce雲端專案的幾種方法。 瞭解如何提取資料庫以使用站外。 瞭解遮罩和移除PII的一些方法。
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-02-14T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: a951f61ff71ad3777f8aebfa3c237b2ec1a4b1a5
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# 連線並針對Adobe Commerce資料庫執行查詢

在本教學課程中，您將瞭解如何連線至雲端專案上的Adobe Commerce、傾印資料庫以供在異地使用，以及遮罩PII並移除它。

您可以使用下列任一方法，從雲端專案存取Adobe Commerce資料：

* 使用本機資料庫傾印
* 使用Mysql Workbench或Tables Plus之類的應用程式來建立與遠端雲端環境的DB連線
* 使用magento-cloud CLI工具直接連線至雲端環境，並在遠端伺服器上執行命令

建議的方法是執行資料庫傾印並擦除它以移除任何客戶資訊。 如果不需要資料，請完全移除客戶資料。

## 使用Adobe Commerce Cloud CLI工具

建立資料庫傾印需要您擁有 [ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) 已安裝。 在本機筆記型電腦上，移至目錄，然後執行下列命令。 請務必取代 `your-project-id` 使用專案ID，類似 `asasdasd45q`. 您也需要取代 `your-environment-name` 使用您環境的名稱，例如 `master` 或 `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

如果您不確定專案ID或環境，可以在命令中省略這些專案：

`magento-cloud db:dump`

CLI會要求您指定正確的專案和環境。 下列範例會顯示該對話方塊。 此範例顯示指派給您帳戶的多個專案，但您可能只有一個可用的專案。

變更至目錄

```bash
cd ~/Downloads/db-tutorial 
```

現在執行命令以建立資料庫傾印

```bash
magento-cloud db:dump
```

由於我們未指定專案或環境，Adobe Commerce CLI將會詢問幾個問題，以下是一些範例對話方塊

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## 使用Adobe Commerce ECE-tools

如果您沒有Adobe Commerce CLI工具，可以 `ssh` 至您的專案並執行 `ece` 命令 `vendor/bin/ece-tools db-dump`：範例回應：

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

使用 `SFTP` 或 `rsync` 將資料庫傾印提取到您的本機環境。

以下範例使用 `rsync` 將檔案提取至 `~/Downloads/db-tutorial` 資料夾。

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

終端機視窗將會輸出一些資訊，以下是一些範例輸出

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

檢視檔案內容，確認已成功下載。

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

取得資料後，請務必移除或遮罩客戶資料，加以清除。 以下範例指令碼將幫助您開始使用。

此範例會將客戶資料轉換為隨機字串，但會保留所有專案。 此範例包含一些額外的表格，以示範可在第三方表格以及核心表格中找到客戶PII。 仔細檢查每個表格中的資料，並遮罩或移除任何客戶資料。

通常，架構師或主要開發人員是負責遮罩和清理資料庫傾印的唯一人員。 配備專屬的淨水器可降低原始資料的曝光率，減少違反合規規則和法規的機會。

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

或者，您可以刪除記錄，而不是遮罩資訊，這樣也會使新的DB變小。 遮罩或移除PII後，資料就可以安全地提供給隊友，以供在本機環境使用。

## 到Adobe Commerce Cloud專案的遠端DB連線

此方法不允許意外編輯和刪除真實資料。 使用此方法時請務必謹慎。 建議使用資料庫備份並離線檢閱資料。 某些情況下需要直接在Adobe Commerce Cloud存取資料，但這確實有風險。 沒有「您確定嗎？」 如有疑問，可能會無意中變更或移除資料。

超重要！ 進行遠端DB連線既方便又使用真實即時資料，但也有風險。 我個人及身為Adobe Commerce的主要技術架構師，不建議這麼做。 很容易忘記您位於遠端DB，並意外刪除或修改資料。 您可以選擇連線到唯讀復本，但是這會根據SQL活動的負載大小對網站造成一些影響。 不過，由於這是可能的，因此以下是完成此目標的步驟。

建立SSH通道：

```bash
magento-cloud tunnel:open
```

選擇專案並挑選環境後，會輸出用於mysql圖形介面設定中的命令。

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

使用MySQL圖形介面建立連線，方法是使用 `SSH tunnel opened to database at` 命令選項。

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

現在您有了正確的資訊，請繼續將這些值插入Cloud Console。

您可以在Cloud Console的雲端憑證中找到SSH主機名稱和使用者名稱。

![標誌 — Adobe Commerce Cloud Console](./assets/cloud-ui-screenshot.png "Adobe Commerce Cloud Console")

以下是其中一個範例： `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
SSH主機名稱是@符號後面的所有專案： `ssh.us-4.magento.cloud` 在此範例中。
SSH使用者名稱是@符號之前的所有專案：  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## 尋找要連線到資料庫的值

直接存取MariaDB資料庫需要使用SSH登入遠端雲端環境並連線至資料庫。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 從擷取MySQL登入認證 `database` 和 `type` 中的屬性 [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) 變數中。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在回應中，尋找MySQL資訊。 例如：

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

然後使用您MySQL GUI中的設定值。 下列範例使用MySQL Workbench，但任何支援MySQL連線的應用程式都會有類似的欄位。

![標誌 — 使用Mysql Workbench的Mysql GUI範例](./assets/mysql-workbench-after-connecting.png " 使用Mysql Workbench的Mysql GUI範例")

![logo — 使用TablesPlus的Mysql GUI範例](./assets/tablesPlus-db-connection.png " 使用TablesPlus的Mysql GUI範例")

完成所有設定後，就可以使用MySQL GUI在遠端Adobe Commerce Cloud專案上執行查詢。

## 直接連線到雲端專案資料庫以執行SQL

以下方法使用 `magento-cloud` cli直接連線至mysql資料庫並執行SQL，以加快資料庫查詢速度。 如果您需要複製此資料庫，請參閱以下其中一種替代方法： [建立資料庫傾印](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

例如，您可以從 `core_config_data` 包含單字的表格 `secure` 做為欄的一部分 `path`：

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## 其他資源

[ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[設定MySQL服務](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[設定遠端MySQL資料庫連線](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[在雲端基礎結構上的Adobe Commerce上建立資料庫傾印](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
