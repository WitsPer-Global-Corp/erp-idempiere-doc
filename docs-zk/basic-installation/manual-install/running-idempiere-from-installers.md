---
sidebar_position: 4
---

# 從安裝程序運行IDEMPIERE

## 手動運行

安裝並配置了IDEMPIERE服務器後，您可以以：

```shell
su - idempiere  # not necessary if you're already as user idempiere
```

```shell
cd /opt/idempiere-server
```

```shell
sh idempiere-server.sh
```

## 安裝為服務

可以將IDEMPIERE註冊為Linux的服務，為此，您可以將提供的腳本複製到`/etc/init.d`文件夾，例如：

```shell
sudo su -    # this must be executed as root
```

```shell
cp /opt/idempiere-server/utils/unix/idempiere_Debian.sh /etc/init.d/idempiere
```

```shell
systemctl daemon-reload
```

```shell
update-rc.d idempiere defaults
```

在將IDEMPIERE註冊為服務後，它將在服務器重新啟動時自動啟動，也可以按照以下方式啟動 /停止 /重新啟動 /檢查以下情況：

```shell
systemctl status idempiere     # to check the status of the app
systemctl restart idempiere    # to restart the iDempiere app
systemctl stop idempiere       # to stop the iDempiere app
systemctl start idempiere      # to start the iDempiere app when stopped
```

注意以上說明是特定於ubuntu或debian的，在文件夾`/opt/idempiere-server/utils/unix`中也有用於redhat和suse的腳本，但是在這些系統上安裝的腳本可能會有所不同。

## 故障排除

### 在Mac OS上運行IDEMPIERE-SERVER.SH時錯誤

如果您嘗試運行腳本並遇到以下錯誤：

```
===================================
Starting iDempiere Server
===================================
readlink: illegal option -- f
usage: readlink [-n] [file ...]
usage: dirname path
Error: Unable to access jarfile /plugins/org.eclipse.equinox.launcher_1.*.jar
```

您必須獲取包含GNU命令行工具的Coreutils軟件包。如果您有啤酒等軟件包經理，則可以通過它獲得Coreutils。

之後，將垃圾箱添加到端子的路徑（bash或zsh或等）。

```shell
export PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH"
```