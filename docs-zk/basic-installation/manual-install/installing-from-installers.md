---
sidebar_position: 3
---

從"@themy/tabs"導入選項卡；
導入tabitem來自"@them/tabitem";

# 安裝程序安裝

## 創建用戶

建議運行IDEMPIERE服務器作為用於此目的（通常是IDEMPIERE）的用戶，而不是作為root運行。

```shell
adduser idempiere
```

：：：警告

請勿將IDEMPIERE作為根。

:::

## 安裝服務器

解開您下載或創建的服務器安裝程序

```shell
jar xvf idempiereServer10Daily.gtk.linux.x86_64.zip
```

將文件夾移至 /opt

```shell
mv idempiere.gtk.linux.x86_64/idempiere-server /opt
```

```shell
rmdir idempiere.gtk.linux.x86_64
```

```shell
chown -R idempiere:idempiere /opt/idempiere-server
```

從現在開始，最好是您以idempiere用戶運行所有內容：

```shell
su - idempiere  # not necessary if you're already as user idempiere
```

```shell
cd /opt/idempiere-server
```

<tabs>
<tabitem value ="圖形" label ="圖形">

您可以運行：

```shell
sh setup.sh
```
或者

```shell
sh setup-alt.sh
```

：：：筆記

您可以選擇添加一個日誌級別參數（可接受的值是：關閉，嚴重，警告，信息，配置，config，fine，finer，finest，最好的，所有）。例如`sh setup-alt.sh fine

:::

您可以如屏幕截圖所示填充參數，或者使用自己的首選值，特別是您必須處理以下內容：

- idempiere Home：這是存儲庫文件夾
- Web端口 / SSL：小心不要使用另一個應用程序已經使用的端口，在1000以下的Linux端口中，非Root用戶無法使用。例如，Oracle-XE使用端口8080
- 數據庫已經存在：在常見的安裝中，您必須不選中此標誌，因為稍後將創建數據庫
- 數據庫名稱：在這裡，我們填寫要創建的數據庫的名稱
- DB管理員密碼：必須使用您在[先決條件]中設置的Postgres密碼（./ install-prerequisites.md＃nocest-a-a-password-to-user-postgres）
- 數據庫用戶：這是要創建的用戶，建議您將其保留為默認的adempiere
- 數據庫密碼：在此處填寫要分配給數據庫的密碼

！ [idempiere服務器設置]（/img/docs/basic-installation/Manual-install/ScreenShot-idempiere_server_server_setup.png）

最後按"保存"按鈕，如果某些內容失敗，將禁用保存按鈕，並且失敗的選項以紅色標記，以便重新啟用保存按鈕，則必須按測試按鈕，直到所有錯誤都消失為止。

當數據庫仍未創建時，紅色唯一的 *有效錯誤 *在數據庫密碼字段的前面。

### Oracle的差異

在Oracle上，一些字段必須略有不同：

- 數據庫名稱：在這裡您必須填寫Oracle實例的名稱（通常是XE或ORCL）
- DB Admin密碼：安裝Oracle時必須使用您設置的系統密碼
- 數據庫用戶：在Oracle您可以在此處定義您的首選用戶
- 數據庫密碼：在此處填寫要分配給數據庫的密碼


</tabitem>
<tabitem value =" console" label =" console">

跑步

```shell
sh console-setup-alt.sh
```

或者

```shell
sh console-setup.sh
```

：：：筆記

您可以選擇添加一個日誌級別參數（可接受的值是：關閉，嚴重，警告，信息，配置，config，fine，finer，finest，最好的，所有）。例如`s sh console-setup-alt.sh fine

:::

當您沒有圖形環境時，將使用此方法，在這種情況下，參數顯示在屏幕中，則必須直接用鍵盤填充它們。

</tabitem>
<tabitem value =" console-silent" label =" console Silent">

運行設置還有另一種非拍照的非相互作用方法。

您需要使用從另一台服務器複製的先前的`iDempiereenv.properties`文件，否則手動填寫從`iDempiereenvtemplate.properties`填寫和替換）。

有一個有效的IDEMPIEREENV.PROPERTIES之後，您可以執行：

```shell
sh silentsetup-alt.sh
```
或者

```shell
sh silent-setup.sh
```

：：：筆記

您可以選擇添加一個日誌級別參數（可接受的值是：關閉，嚴重，警告，信息，配置，config，fine，finer，finest，最好的，所有）。例如`s silent-stetup-alt.sh fine'

:::

當您沒有圖形環境並且是非相互作用的情況下，該程序將用於使用這一點，該程序從文件IDEMPIEREENV.PROPERTIES中讀取所有值並使用該值配置系統。

</tabitem>
</tabs>

## 導入數據庫

這是導入Oracle（> = 12c）和PostgreSQL（> = 10）的數據庫的默認方法：

設置服務器（是先決條件）後，您可以運行：

```shell
su - idempiere  # not necessary if you're already as user idempiere
```

```shell
cd /opt/idempiere-server/utils
```

```shell
sh RUN_ImportIdempiere.sh
```

**注意：為了使上述導入腳本正常工作，您需要確保路徑上的jar和psql或oracle可執行文件。 **

**關於Oracle的重要說明：為了導入種子數據庫，需要創建指向"數據/種子"的目錄對象，以使DBA組正常工作，需要訪問該文件夾。腳本`utisl/oracle/exportIdempiere.sh`提供此訪問使用'CHGRP dba'，除非IDEMPIERE用戶是DBA組的成員，否則這是行不通的，因此此指令是Oracle的先決條件：
`usermod -g dba idempiere'
此組必須訪問``iDempiere_home`文件夾''，否則您會發現嘗試導入的錯誤**

## 更新數據庫

為了使數據庫與代碼同步，需要運行以下腳本需要：

```shell
su - idempiere  # not necessary if you're already as user idempiere
```

```shell
cd /opt/idempiere-server/utils
```

```shell
sh RUN_SyncDB.sh
```

## 在數據庫中註冊版本代碼

為了在服務器上運行的版本代碼簽署數據庫（或根據配置所需）運行以下腳本：

```shell
su - idempiere  # not necessary if you're already as user idempiere
```

```shell
cd /opt/idempiere-server
```

```shell
sh sign-database-build-alt.sh
```