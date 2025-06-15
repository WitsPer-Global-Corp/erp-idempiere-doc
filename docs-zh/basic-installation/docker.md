---
sidebar_position：2
---

從“@themy/tabs”導入選項卡；
導入tabitem來自“@them/tabitem”;

＃docker

＃＃ 目的

此頁面的目的是幫助您與Docker一起啟動並運行。有關更高級的idempiere docker主題，請參見[idempiere-docker]（https://github.com/idempiere/idempiere-docker）github頁面。

## Docker快速開始

````殼
docker run -d -name postgres -p 5432：5432 -e postgres_password = postgres postgres：15
````````

````殼
Docker Run -d -name Idempiere -P 8443：8443 -Link Postgres：Postgres idempiereOfficial/idempiere：11 realeles
````````

## Docker撰寫快速開始

創建一個`docker-stack.yml`文件：

``yaml
版本：'3.7'

服務：
idempiere：
圖片：IDEMPIEREROFFICIAL/IDEMPIERE：11點釋放
卷：
-Idempiere_config：/opt/idempiere/configuration
-Idempiere_plugins：/opt/idempiere/插件
環境：
-TZ =美國/芝加哥
端口：
-8080：8080
-8443：8443
-12612：12612

Postgres：
圖片：Postgres：15
卷：
-Idempiere_data：/var/lib/postgresql/數據
環境：
-TZ =美國/芝加哥
-postgres_password = Postgres
端口：
-5432：5432

卷：
idempiere_data：
idempiere_plugins：
idempiere_config：

````````

Docker組成：

``bash
$ docker compose -f docker -stack.yml up
````````