---
description: 本章主要指导如何使用docker快速搭建superset生产环境，以及安装clickhouse连接器驱动。
---

# Supserset

### 1.克隆 superset

```
git clone https://github.com/apache/superset.git
```

### 2.在.env-local中添加环境变量&#x20;

```
cd superset/docker

cat >> .env-local << EOF
SUPERSET_ENV=production
SUPERSET_LOAD_EXAMPLES=no
EOF

#配置密匙
echo export SUPERSET_SECRET_KEY="`(openssl rand -base64 42)`" >> .env-local 
```

.env-local 会覆盖.env中的默认配置

### 3.修改superset\_config.py ，修改默认语言为中文

```
cd /pythonpath_dev
vim superset_config.py
```

加入配置

```
BABEL_DEFAULT_LOCALE = "zh"
LANGUAGES = {
    "zh": {"flag": "cn", "name": "简体中文"},
    "en": {"flag": "us", "name": "English"},
} 
```

### 4.创建Dockerfile，安装clickhouse公司的python驱动

对于不同的数据库，参考https://superset.apache.org/docs/databases/installing-database-drivers

```
FROM apache/superset:4.0.2-py310
USER root
RUN pip install clickhouse-connect -i https://pypi.tuna.tsinghua.edu.cn/simple
```

写这篇文章时，superset版本目前是4.0.2，读者根据自己当前版本选择

### 5.构建新的镜像

```
docker build -t tongxin/superset-clickhouse:4.0.2 .
```

### 6.回到superset目录，修改docker-compose-non-dev.yml

{% file src="../../.gitbook/assets/docker-compose-non-dev.yml" %}

改动如下，删除红色区域的部分，添加绿色部分。也可以使用上面我修改好的文件。

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### 7.启动容器

```
docker compose -f docker-compose-non-dev.yml up
```

在浏览器访问8088端口，账号密码为 admin/admin





