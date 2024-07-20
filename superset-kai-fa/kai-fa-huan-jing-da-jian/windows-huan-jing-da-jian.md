---
description: github上下载源码在本地解压
---

# Windows 环境搭建

## 后端

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
python superset db upgrade

# 创建管理员用户
 fabmanager create-admin --app superset

# 创建默认角色和权限
python superset init

# 加载案例数据 ，在此之前必须创建管理员用户
python superset load_examples

# 启动 Flask 
flask run -h 0.0.0.0 -p 8088 --with-threads --reload --debugger
```



在`pip install -r requirements/development.txt`安装依赖时报错：

```
error: Microsoft Visual C++ 14.0 or greater is required. 
Get it with “Microsoft C++ Build Tools”: https://visualstudio.microsoft.com/visual-cpp-build-tools/ 
```

安装C++ buildTools ，下载地址为[http://go.microsoft.com/fwlink/?LinkId=691126](http://go.microsoft.com/fwlink/?LinkId=691126)



**如果安装了C++ buildTools还是报错，那就只能visual studio全家桶了**

https://visualstudio.microsoft.com/visual-cpp-build-tools/下载构建工具并安装。

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

将相应的组件安装好后再运行一次



## 前端

建议使用node17或18版本

想要快点的话使用cnpm去下载

<pre><code><strong>#安装cnpm
</strong><strong>npm install -g cnpm --registry=https://registry.npm.taobao.org
</strong>
#配置淘宝源
npm config set registry https://registry.npm.taobao.org --global

</code></pre>







