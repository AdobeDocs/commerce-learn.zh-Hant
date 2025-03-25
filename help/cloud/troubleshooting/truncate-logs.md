---
title: 截斷記錄
description: 瞭解如何截斷大型記錄檔，以因硬碟已滿而分類失敗的部署。
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
source-git-commit: b90aa9eb8759391a16dfb29ca25b0d2d271956ed
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# 截斷記錄

瞭解如何分類以及因硬碟已滿導致的部署失敗。 瞭解如何尋找以及可以執行哪些命令來釋放Adobe Commerce雲端環境中的空間。

如果您認為您可能需要這些記錄檔，您可以`rsync`它們，或在截斷它們之前，使用其他方法取得伺服器上可用的復本。

## 此影片給誰看

- 開發人員與IT專業人員
- 系統管理員

## 視訊內容

- 診斷並解決失敗的部署
- 找到一些常見的大型記錄檔
- 截斷記錄檔的快速方法

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## 視訊中使用的命令

檢查硬碟空間`df -h`。 請留意行dev/mapper/xxxx

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


使用命令`ls -lah`以人類可讀的格式（例如kb、mb和gb）顯示檔案及其大小

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## 截斷記錄的範例

ssh連線至正確的專案和環境後，請變更至`var/log`目錄。 然後，您可以使用類似於`> some-log-file.log`的檔案來截斷檔案

```BASH
> support_report.log 
> cron.log 
```

## 相關檔案

- [健康狀態通知](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
