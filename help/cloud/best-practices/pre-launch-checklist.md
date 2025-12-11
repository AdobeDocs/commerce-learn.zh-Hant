---
title: Adobe Commerce Cloud啟動前檢查清單
description: 瞭解Adobe Commerce Cloud啟動前的檢查清單。
feature: Cloud
topic: Commerce, Architecture, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1668'
ht-degree: 0%

---

# Commerce Cloud啟動前檢查清單

以下是Adobe Commerce [網站啟動檔案](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}的摘要。

此檢查清單旨在協助規劃及執行Adobe Commerce Cloud網站的成功啟動。 與您的Adobe Commerce Cloud系統整合商合作，確保所有設定任務和檢查清單專案都完成並驗證。 如果您遇到任何檢查清單專案困難或有疑問，請聯絡指定的客戶技術顧問或客戶成功工程師。 如果您的帳戶未獲指派CTA/CSE，您可以建立支援票證以尋求協助。

如果您的CTA/CSE已指派給帳戶，請在啟動新的Adobe Commerce Cloud網站至少4週前聯絡他們和客戶經理，以通知他們您的&#x200B;**意圖**&#x200B;啟動。

- 有些檢查會以[!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}標示出來，因為如果不仔細檢閱，可能會封鎖您的上線。
- 確保與您的開發人員或系統整合合作夥伴共同作業，以符合您的實作方法。

>[!IMPORTANT]
> 如果您未使用並完成此檢查清單，您接受[責任](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}對於生產啟動排程和持續性網站穩定性的任何不利影響和相關風險。

## 1.上線前

1. 檢閱有關測試和上線的檔案[網站啟動檔案](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >確保與您的合作夥伴或系統整合商一起完全準備完整的&#x200B;_「上線整備計畫」_，並納入所有必要的行動專案。 請記住，雖然啟動前檢查清單強調Adobe的最佳實務，但&#x200B;_&#x200B;**並不**&#x200B;_&#x200B;取代您自己的上線整備計畫。

2. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}檢閱支援深入分析(SWAT)建議與資訊（[使用手冊](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"}）
3. 一般使用者/商家執行UAT （使用者驗收測試），包括後端作業。
4. 系統整合員團隊已在中繼和生產環境中執行端對端UAT。 請參閱[Experience League檔案](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}。
5. 確認在中繼和生產環境中部署及測試程式碼（[瞭解詳情](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}）。
6. 生產叢集的大小已永久增加至合約的每日基準線。 與指派的CTA/CSE洽談以取得詳細資訊，或提出支援票證。

## 2.目前的設定

1. 將Adobe Commerce和相關套件/服務升級至[最新版本](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/release/notes/overview){target="_blank"}
2. 與您的SI/合作夥伴檢閱目前的設定和服務，然後[遵循最佳實務](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}。
3. 檢閱MySQL/Shared-Files [磁碟使用量](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## &#x200B;3. Fastly設定

1. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}確定快取運作正常([整頁快取](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"}或[GraphQL快取](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"})。 閱讀[Fastly設定指南](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}。
2. 在PWA/Headless網站上使用GET方法進行GraphQL查詢（如適用）。

   >[!NOTE]
   > 只有使用HTTP GET操作提交的查詢才能進行快取（如果適用）。 無法快取[POST查詢](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}。

3. 確定已啟用Fastly影像最佳化（[請參閱Fastly影像最佳化](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"}）
4. 確認已設定正確的遮蔽位置（[設定快取、後端和來源遮蔽](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}）。
5. Web應用程式防火牆(**WAF**)正在運作。 (請參閱[疑難排解封鎖的請求](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"} （如果有的話）和限制)
6. 更新Admin面板中的Fastly [「忽略的URL引數」](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"}清單，以增強快取效能。

   >[!NOTE]
   > 在&#x200B;_管理員>存放區>設定>系統>完整頁面快取> Fastly設定>進階設定>忽略的URL引數（全域）_&#x200B;底下的Fastly設定中，您可以找到Fastly在搜尋快取頁面時應忽略的逗號分隔引數清單。 請務必在修改此清單後重新上傳VCL

## &#x200B;4. DNS和SSL

1. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}確認已要求所有必要的網域名稱。 _（針對任何新增或變更的網域預先提交支援票證）_
2. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"} SSL (TLS)憑證已套用至網域。 閱讀[本文章](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"}以取得詳細資訊。
3. 將DNS [TTL （存留時間）](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"}值更新到最小值，以便上線。
4. 啟用Sendgrid SPF和DKIM

   >[!NOTE]
   > 將每個網域的SendGrid CNAME記錄新增至DNS設定。 閱讀[SendGrid電子郵件服務](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}以瞭解如何變更寄件者網域等。

## 5.資料庫組態

Adobe Commerce Cloud採用MariaDB Galera叢集作為中繼和生產環境的資料庫。 Galera叢集有助於增強效能與擴充性。 若要深入瞭解Galera叢集複製的最佳實務和限制，請參閱下列文章。

- [MySQL設定最佳實務](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Adobe Commerce上的受管理警示： [MariaDB警示](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- [資料庫組態](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}的最佳實務
- 深入分析[Galera叢集復寫與流量控制。](https://experienceleague.adobe.com/zh-hant/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. 建議使用[MYSQL從屬連線](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"}，以便在資料庫負載高時提升效能。
2. 確定所有資料庫資料表的資料列格式皆設為[DYNAMIC，而非COMPACT](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} （對於內部部署至雲端移轉尤其如此）。
3. 將所有資料表的資料庫儲存體引擎從[MyISAM變更為InnoDB](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"}。
4. 提前檢閱和最佳化大小超過1 GB的資料庫表格。
5. 資料庫結構描述資訊是最新的資訊。 （請參閱[本指南](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}）。

## 6.部署

1. 檢閱靜態內容部署(SCD)理想狀態，以縮短在生產環境中進行部署期間的維護時間。 檢閱[靜態內容部署(SCD)策略](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"}和[存放區組態管理](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}指南。
2. 檢閱HTML、JavaScript和CSS的縮制設定。 (這不適用於PWA/Headless網站)。
3. 確認下列雲端變數的使用方式與其預期目的一致。 （[SCD_MATRIX](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}、[SCD_ON_DEMAND](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"}和[SKIP_SCD](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"}）

## 7.測試與疑難排解

1. 測試傳出異動電子郵件。 深入瞭解[Adobe Commerce Cloud - SendGrid Mail功能](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}。
2. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}是否有Adobe的封鎖程式？
3. [!BADGE 封鎖程式]{type=caution tooltip="潛在的阻斷因素"}在投入使用前對生產執行個體執行負載和壓力測試，並與指派的CTA/CSE共用結果。

   >[!NOTE]
   > [負載和壓力測試的目的](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}在於識別應用程式內的瓶頸並找出效能問題。 在管理有關叢集規模的期望以及決定必要的規模調整以有效滿足業務需求方面，它起著關鍵的作用。

   >[!IMPORTANT]
   > **_WARNING:_**&#x200B;在準備負載測試時，請_ **_不要_**&#x200B;傳送即時交易電子郵件（即使是虛擬位址）。 在測試期間傳送電子郵件，可能會使專案達到在啟動之前為SendGrid設定的預設傳送限制(12k)。
   > 
   > - 如何停用電子郵件通訊：
   >   移至&#x200B;_商店>設定>進階>系統>電子郵件傳送設定_。

4. 在[共擔責任安全性模型](https://business.adobe.com/tw/products/magento/secure-ecommerce.html){target="_blank"}中，對生產執行個體進行安全性滲透測試。 為了符合PCI （支付卡產業）規範，客製化網站需要進行滲透測試。

## 8.其他組態

1. 將索引切換為排程&#x200B;_上的_&quot;更新&quot;，但&#x200B;**_customer_grid_**&#x200B;保留在&quot;SAVE&quot;上（請參閱[索引模式](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}）。
2. 您使用任何協力廠商搜尋引擎或擴充功能嗎？
3. 確認[SEO （搜尋引擎最佳化）設定已正確設定](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"}，以啟用索引子/編目程式掃描網站（若相關）。
4. 新增重新導向與路由（請參閱[設定路由](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"}）

   >[!NOTE]
   >在整合環境中將重新導向和路由新增至routes.yaml檔案，並在部署至中繼和生產環境之前驗證此環境中的設定。

       &quot;http://{all}/&quot;：
       型別：上游
       上游： &quot;mymagento：http&quot;
       
       &quot;http://{all}/&quot;：
       型別：上游
       上游：&quot;mymagento：http&quot;
   
5. 如果在開發期間啟用，請確定XDebug已停用（請參閱[設定Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}）。
6. 請確認php.ini檔案中的op-cache和其他設定已正確更新（[請參考此範例](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}）。
7. 訂閱&#x200B;[**Adobe Commerce狀態頁面**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}。
8. 訂閱Adobe Commerce [通知通道的New Relic &#x200B;](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}受管理警報，以監視指定的效能量度（[詳細資訊](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}）。

## 9.安全性

1. 設定Adobe Commerce安全性掃描

   >[!NOTE]
   > [Adobe Commerce安全性掃描是很有用的工具](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/security/security-scan){target="_blank"}，可協助您找出網站上的過時軟體版本、不正確的設定以及潛在的惡意軟體。 註冊、安排經常執行，並確保將電子郵件傳送給正確的技術安全連絡人。
   > 
   > 在UAT期間完成此工作。 如果您使用定期掃描選項，請務必在低需求時間排程掃描。 請參閱Adobe Commerce帳戶中的[安全性掃描](https://account.magento.com/scanner/index/dashboard/){target="_blank"}頁面。 您必須登入Adobe Commerce帳戶才能存取安全性掃描。

2. 變更Adobe Commerce管理員的預設設定。
3. 變更管理員密碼（請參閱[設定管理員安全性](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/security/security-admin){target="_blank"}）。
4. 變更管理員URL （請參閱[使用自訂管理員URL](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}）。
5. 移除專案中任何已不存在的使用者（請參閱[建立及管理使用者](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}）。
6. 已設定管理員的密碼（請參閱[管理員密碼需求](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/security/security-admin){target="_blank"}）。
7. 設定雙因素驗證（請參閱[雙因素驗證](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}）。

## 10.上線

當要切換時，請執行以下步驟（如需詳細資訊，請參閱[DNS設定](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}）：

1. 存取您的DNS服務，並更新每個網域和主機名稱的A和CNAME記錄：
   1. 新增&#x200B;_&lt;&lt;www.yourdomain.com>>_&#x200B;的CNAME記錄，指向&#x200B;**prod.magentocloud.map.fastly.net**
   2. 為&#x200B;_&lt;&lt;yourdomain.com>>_&#x200B;設定四條A記錄，指向：\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. 將Adobe Commerce基底URL變更為&#x200B;_&lt;&lt;www.yourdomain.com>>_
3. 請等候TTL時間過去，然後重新啟動網頁瀏覽器。
4. 測試網站。

### 如果您在封鎖上線時發生問題：

如果您遇到任何問題，導致在轉換期間無法啟動應用程式，取得適當即時支援的最快方法就是利用服務檯並開啟票證，理由為「無法啟動我的商店」，然後撥打熱線支援號碼(請參閱[Adobe Commerce P1 （優先順序1）熱線號碼清單](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"})：

- 美國免付費電話： (+1) 877 282 7436 (直接前往Adobe Commerce P1熱線)
- 美國免付費電話： (+1) 800 685 3620 (第一個功能表按7 Adobe Commerce P1熱線電話)
- 美國當地： (+1) 408 537 8777

## 11.上線後

一旦網站上線，請傳送電子郵件給指派的CTA （客戶技術諮詢）、CSE （客戶成功工程師）和AM （客戶經理）。 但是，如果您沒有指派帳戶管理員給專案，您可以建立支援票證，要求一旦網站上線，啟用高SLA監控。 一旦網站經驗證可透過啟用Fastly的快取啟動，CTA/CSE就會執行以下工作：

- 將叢集標籤為已上線並建立支援票證以啟動高SLA （服務等級協定）監視。
- 啟動New Relic Synthetics以監控運作時間。

>[!MORELIKETHIS]
> 
> - [Launch整備概覽 — 實作行動手冊](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [啟動檢查清單 — 使用手冊](https://experienceleague.adobe.com/zh-hant/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [啟動前檢查清單 — 網站管理員/Commerce管理員指南](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [共用職責安全性模型](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
