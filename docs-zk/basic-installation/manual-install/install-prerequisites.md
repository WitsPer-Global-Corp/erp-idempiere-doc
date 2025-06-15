---
sidebar_position: 1
---

從"@themy/tabs"導入選項卡；
導入tabitem來自"@them/tabitem";

# 安裝先決條件

本指南上的示例使用以下版本：
- Ubuntu 22.04 64位
- Postgresql 14
- OpenJDK 17

：：：筆記

為了運行Idempiere，您需要擁有JDK（不是JRE）Java的版本

:::

:::資訊

idempiere也可以使用Oracle 12c及以後運行，以及以後的Postgresql 11及以後，對於本教程，我們使用PostgreSQL 14

:::

## Ubuntu

請參閱http://www.ubuntu.com/download

下載並安裝了Ubuntu Server 22.04 LTS

## Postgresql 14

### 安裝

創建文件存儲庫配置：
```shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

導入存儲庫簽名密鑰：
```shell
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

更新軟件包列表：
```shell
sudo apt-get update
```

安裝PostgreSQL的版本14。
```shell
sudo apt-get -y install postgresql-14
```

：：：筆記

在其他版本的Linux中，您需要另外安裝與您安裝的版本相對應的PostgreSql-Contrib包。在Ubuntu中默認包含在Ubuntu中，但在其他情況下必須安裝它，否則您將獲得有關generate_uuid（）函數的錯誤。
另外，對於其他Linux版本而言，指令有所不同，在某些版本中，您需要創建和啟動群集，並且還為自動啟動配置了服務。請參閱有關此類情況的特定[PostgreSQL]（https://www.postgresql.org/download/）指令。

:::

### 將密碼分配給用戶Postgres

為了創建數據庫，安裝程序需要知道用戶Postgres的密碼，默認情況下，該用戶在Ubuntu中沒有密碼（Windows Installer要求提供密碼）。

請注意您在此處分配的密碼，因為在設置過程中需要需要：

步驟是（將your_chosen_password替換為您的首選）：
```shell
echo "alter user postgres password 'your_chosen_password'" | sudo su postgres -c "psql -U postgres"
```

### 配置pg_hba.conf

安裝Postgres後，您必須檢查pg_hba.conf的正確配置

以下行需要更改身份驗證方法：

<tabs>
<tabitem value =" ubuntu" label =" ubuntu">

```shell title="/etc/postgresql/14/main/pg_hba.conf"
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
# highlight-next-line
local   all             all                                     peer
```

更改為：
```shell title="/etc/postgresql/14/main/pg_hba.conf"
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
# highlight-next-line
local   all             all                                     scram-sha-256
```

</tabitem>
<tabitem value =" Windows" label =" Windows">

```shell title="C:\Program Files\PostgreSQL\14\data\pg_hba.conf"
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
# highlight-next-line
local   all             all                                     peer
```

更改為：
```shell title="C:\Program Files\PostgreSQL\14\data\pg_hba.conf"
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
# highlight-next-line
local   all             all                                     scram-sha-256
```

</tabitem>
</tabs>

：：：筆記

一些指南建議配置信任而不是MD5-但這在您的Postgres服務器上創建了安全問題。

:::

然後重新加載配置：
```shell
sudo service postgresql reload
```

## OpenJDK

```shell
sudo apt-get update
```

```shell
sudo apt-get install openjdk-17-jdk-headless
```

：：：筆記

如果您打算使用UI程序安裝OpenJDK-17-JDK，則OpenJDK-17-JDK-Headless適用於沒有GUI的服務器。另請注意，OpenJDK-17-JRE還不夠，因為它在某些腳本中沒有JAR命令。

:::

：：：筆記

您還可以安裝OpenJDK> 17，我們建議使用17歲這樣的長期支持的長期支持。

:::

## 故障排除

### 沒有Postgres用戶

如果您的Postgres DB中沒有" Postgres"角色，則必須使用以下步驟自己創建：

通過PSQL連接到Postgres DB實例。
```shell
psql
```

創建" Postgres"角色
```sql
CREATE USER postgres
```

改變角色並指定角色應該是什麼。
```sql
ALTER USER postgres SUPERUSER CREATEDB
```

### CentOS上的InvocationTargetException

在具有最低版本的CentOS服務器上，您可以在主頁上獲得錯誤（通過渲染圖）

建議的解決方案：
```shell
sudo apt install fontconfig
```

```shell
sudo yum install fontconfig
```

### 視窗

Windows中的大多數安裝問題都是由未配置的環境變量引起的：
- 必須配置路徑以使命令psql和jar可訪問
- java_home環境變量必須配置
