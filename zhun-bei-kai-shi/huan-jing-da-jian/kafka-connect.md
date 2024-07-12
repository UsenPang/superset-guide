---
description: 本章主要指导如何部署kafka connect，并配置数据复制复制，以postgres复制数据到clickhouse为例。
---

# kafka connect

{% file src="../../.gitbook/assets/clickhouse-sink-connect.tar" %}

### 1.解压文件

下载上面的文件，在你的Linux系统中使用命令`tar -xvf clickhouse-sink-connect.tar`解压.

### 2.启动容器

```
cd clickhouse-sink-connect

docker compose up
```

### 3.配置postgresql

在注册连接器之前查看你的数据库中是否启动了Debizium所需的CDC相关配置，这里主要介绍postgres的配置。其他的可以查看官方文档，如[Mysql设置](https://debezium.io/documentation/reference/2.5/connectors/mysql.html#setting-up-mysql)。

配置**postgresql.conf**文件

```
# MODULES
shared_preload_libraries = 'pgoutput' #使用官方的逻辑解码插件

# REPLICATION
wal_level = logical         #Postgres 预写日志中使用的编码类型
max_wal_senders = 10        # 用于处理 WAL 更改的最大进程数，根据需要调整
max_replication_slots = 10  # 允许流式传输 WAL 更改的最大复制槽数，根据需要调整
wal_keep_size = 1024        # 根据需要调整，单位为MB
```

### 4.注册pg连接器

进入script目录编辑postgres-source-config.sh

```sh
# Postgres config
SOURCE_CONNECTOR_NAME="postgres-source-connector"
POSTGRES_HOST="host" #postgres服务器ip
POSTGRES_PORT=5432   #端口号
POSTGRES_USER="user" #用户
POSTGRES_PASSWORD="password" #密码
DATABASE_DBNAME="database"  #源数据库
TOPIC_PREFIX="postgres"
```

注册连接器

```
./postgres-source-register.sh postgres-source-config.sh
```



### 5.注册clickhouse连接器

进入script目录编辑clickhouse-sink-config.sh

```bash
# Clickhouse config
SINK_CONNECTOR_NAME="postgres-sink-connector"
CLICKHOUSE_HOST="host"  #postgres服务器ip
CLICKHOUSE_PORT=8123    #端口号
CLICKHOUSE_USER="user"  #用户
CLICKHOUSE_PASSWORD="password"  #密码
CLICKHOUSE_DATABASE="database"  #目标数据库
TOPICS_REGEX="postgres.*"
```

注册连接器

```
./clickhouse-sink-register.sh clickhouse-sink-config.sh
```



### 6.登录到kafka UI

在浏览器访问8089端口会看到你在kafka中的topic

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>



### 7.在kafka UI中创建连接器

上面是基于脚本来创建连接器，也可以在UI界面创建连接器

