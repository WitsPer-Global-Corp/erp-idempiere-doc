---
sidebar_position: 2
---

從“@themy/tabs”導入選項卡；
導入tabitem來自“@them/tabitem”;

# Docker

## 目的

此頁面的目的是幫助您與Docker一起啟動並運行。有關更高級的idempiere docker主題，請參見[idempiere-docker]（https://github.com/idempiere/idempiere-docker）github頁面。

## Docker快速開始

```shell
docker run -d --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres:15
```

```shell
docker run -d --name idempiere -p 8443:8443 --link postgres:postgres idempiereofficial/idempiere:11-release
```

## Docker撰寫快速開始

創建一個`docker-stack.yml`文件：

```yaml
version: '3.7'

services:
  idempiere:
    image: idempiereofficial/idempiere:11-release
    volumes:
      - idempiere_config:/opt/idempiere/configuration
      - idempiere_plugins:/opt/idempiere/plugins
    environment:
      - TZ=America/Chicago
    ports:
      - 8080:8080
      - 8443:8443
      - 12612:12612

  postgres:
    image: postgres:15
    volumes:
      - idempiere_data:/var/lib/postgresql/data
    environment:
      - TZ=America/Chicago
      - POSTGRES_PASSWORD=postgres
    ports:
      - 5432:5432

volumes:
  idempiere_data:
  idempiere_plugins:
  idempiere_config:

```

Docker組成：

```bash
$ docker compose -f docker-stack.yml up
```
