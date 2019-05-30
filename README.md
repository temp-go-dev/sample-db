# sample-db
サンプルアプリ用のRDB（MySQL）  

## 構成

```
sample-db
  |-- localhost  // ローカルDBをDockerで立ち上げるセット
  |      |-- mysql
  |      |     |-- dbinit.sql
  |      |     |-- Dockerfile
  |      |     `-- my.cnf
  |      `-- docker-compose.yaml
  |
  |-- model
  |      `-- sampmledb.mwb // 設計Model
  |-- script
  |      |-- 00000_init.up.sql
  |      |-- 00000_init.down.sql  
  |      |-- ...
```

## ローカル開発用RDB起動

- 前提
  - Docker for Windowsがインストールされていること
    - Docker
    - Docker-Compose

- DB起動

```bash
$ cd localhost
$ docker-compose up -d

// omitted

$ Creating localhost_mysql_1 ... done
$ docker-compose ps
docker-compose ps
      Name                    Command                State               Ports
---------------------------------------------------------------------------------------
localhost_mysql_1   docker-entrypoint.sh mysqld   Up (healthy)   0.0.0.0:3306->3306/tcp
$
```

- マイグレーション

```bash
$ cd sample-db
$ docker run -v "%CD%\script:/migrations" --network host  migrate/migrate -path=/migrations/ -database mysql://user:password@tcp(localhost:3306)/sampledb up
```
