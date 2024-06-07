---
description: github上下载源码在本地解压
---

# Windows 环境搭建

环境依赖：python3.9、python3.10或python3.11

```bash
# 创建一个虚拟环境并激活
python3 -m venv venv 
source venv/bin/activate

# 安装外部依赖
pip install -r requirements/development.txt

# 以可编辑（开发）模式安装 Superset
pip install -e .

# 初始化数据库
superset db upgrade

# 创建管理员用户
superset fab create-admin

# 创建默认角色和权限
superset init

# 加载案例数据 ，在此之前必须创建管理员用户
superset load-examples

# 从虚拟环境内部启动 Flask 开发 Web 服务器。
superset run -p 8088 --with-threads --reload --debugger --debug
```

在`pip install -r requirements/development.txt`安装依赖时报错：

```
error: Microsoft Visual C++ 14.0 or greater is required. 
Get it with “Microsoft C++ Build Tools”: https://visualstudio.microsoft.com/visual-cpp-build-tools/ 
```

根据提示到https://visualstudio.microsoft.com/visual-cpp-build-tools/下载构建工具并安装。

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

将相应的组件安装好后再运行一次







