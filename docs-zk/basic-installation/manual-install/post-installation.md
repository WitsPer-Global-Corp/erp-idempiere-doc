---
sidebar_position: 5
---

# 後安裝

現在，您已經配置了服務器，建議遵循一些安全措施。

如果您的服務器不向Internet開放，或者是演示服務器，則可能並不重要。但是，如果您的服務器包含生產數據，請警告IDEMPIERE默認配置太開放了，並且必須確保它。

## 推薦的最小步驟

- 來自舊版本時，請查看[遷移說明]（https://wiki.idempiere.org/en/migration_notes）
- 安裝HTTP服務器以用於IDEMPIERE的代理 - 大多數是Nginx或Apache。請參閱[通過nginx]（https://wiki.idempiere.org/en/proxy_idempiere_through_nginx）
    - 配置您的代理以發布 /webui -dempiere默認情況
    - 如果您打算外部使用WebServices，則必鬚髮布 /Ad​​​​interface
    - 如果您計劃在外部使用REST Web服務，則需要發布/api/v1，但是，在這種情況下，建議使用API​​​​網關，例如[krakend]（https：//wiki.idempiere.org/en/proxy_idempiempiempiempiempiere-rest_through_krakend forkrakend forker forker fork k. krakend]提供的示例（示例）
- 使用防火牆關閉服務器上的關閉端口，建議僅打開端口https/443，也許以受保護的方式打開您可能需要的其他端口（即SSH/22）
- 另一個通常的選擇是在VPN後面安裝服務器
- [更改默認密碼]（https://wiki.idempiere.org/en/reset_pass_password_（form_id-200001））用於5個默認用戶（superuser / system / gardenadmin / gardendmin / gardenuser / webService）
- [啟用哈希廣告]（https://wiki.idempiere.org/en/convert_pass_passwords_to_hashes_（process_id-53259））
- 請參閱[確保IDEMPIERE]（https://wiki.idempiere.org/en/securing_idempiere#initial_steps）中的更多建議

## 保持最新

大多數情況下，您可以使用三個簡單的說明使您的idempiere保持最新狀態。

請在啟動之前對IDEMPIERE安裝文件夾進行備份，這很有用，以防更新過程找到問題以避免完整重新安裝。

備份數據庫也很重要。末尾的run_syncdb過程無法回滾。

```shell
# stop the server
cd $IDEMPIERE_HOME   # change to the folder where your iDempiere is installed, usually recommended /opt/idempiere-server
bash update.sh https://jenkins.idempiere.org/job/iDempiere11/ws/org.idempiere.p2/target/repository/
# this URL is for 11 - if you want to keep up to date with master (a.k.a 12 Development Build) then use:
# bash update.sh https://jenkins.idempiere.org/job/iDempiere/ws/org.idempiere.p2/target/repository/
bash sign-database-build-alt.sh
cd utils
bash RUN_SyncDB.sh
# in case of errors, fix the error and execute RUN_SyncDB.sh again until no errors are shown
# at the end start again the server
```

## CentOS最小實例的字體

當您在最小CentOS實例（例如在GCLOUD上）在圖表上運行服務器時，或"性能測量設置"可以通過在服務器上生成並轉換為映像以顯示顯示的"性能測量設置"工作流程，而服務器幾乎錯過了字體。您可以安裝一些字體來解決它

```shell
sudo yum -y install liberation-serif-fonts liberation-sans-fonts liberation-fonts liberation-fonts-common liberation-narrow-fonts liberation-mono-fonts dejavu-lgc-serif-fonts dejavu-serif-fonts dejavu-fonts-common dejavu-lgc-sans-mono-fonts dejavu-sans-fonts dejavu-sans-mono-fonts gnu-free-fonts-common gnu-free-serif-fonts gnu-free-sans-fonts gnu-free-serif-fonts
```