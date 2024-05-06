# 环境搭建

环境的部署主要分为两部分，Superset部分和数据复制部分。使用Docker部署，所以在部署前需要一定的Docker知识，并确保你已经安装了docker,可以参考[这篇文章](ubuntu-22.0.4-huan-jing-pei-zhi.md#id-5-an-zhuang-docker)。

### 数据复制

{% file src="../.gitbook/assets/clickhouse-sink-connect.tar" %}
clickhouse-sink-connect
{% endfile %}

#### 解压文件

数据复制部分主要是debezium、kafka、clickhouse-sink-connect的部署。下载上面的文件，在你的Linux系统中使用命令tar -xvf clickhouse-sink-connect.tar解压.在目录中你会看到以下文件：

```sh
drwxr-xr-x  4 root root   45 Apr 12 05:42 docker/
-rw-r--r--  1 root root 3491 Apr 12 14:29 docker-compose.yaml
-rw-r--r--  1 root root 1613 Apr 12 05:42 README.md
drwxr-xr-x  2 root root  139 Apr 15 10:15 scripts/
```

**docker 目录**:  主要包含一些Dockerfile，用来构建自己的镜像，在Dockerfile中添加了一些额外的东西到官方镜像中，可以不用管它。&#x20;

**docker-compose.yaml** : docker通过这个配置文件快速编排启动多个容器，容器的启动配置都包含在里面。

**scripts 目录**： 这个目录中包含了一些脚本，是用来快速注册连接器。注册连接器也可以在web页面中注册。

#### 启动容器

在clickhouse-sink-connect目录中使用以下命令

```bash
docker compose up
```

注意查看命令行中是否输出报错信息



#### 注册连接器

在注册连接器之前查看你的数据库中是否启动了Debizium所需的CDC相关配置，这里主要介绍postgres的配置。其他的可以查看官方文档，如[Mysql设置](https://debezium.io/documentation/reference/2.5/connectors/mysql.html#setting-up-mysql)。





