# 环境搭建

## 克隆仓库

首先，在 GitHub 上分叉存储库，然后克隆它。

其次，您可以直接克隆主存储库，但无法发送拉取请求。

```
git clone https://github.com/apache/superset.git
cd superset
```

## docker-compose 启动

```
docker compose up
```

这将拉取/构建 docker 映像并运行服务集群，并将本地代码挂在到容器中，所以在本地的代码修改都会被映射到容器中，使容器更新。



## 本地部署

### Flask Server

首先确保你的python版本是3.9、3.10或3.11，然后执行：

<pre class="language-bash"><code class="lang-bash">#安装一些必要的依赖，不然在执行下面命令pip install -r requirements/development.txt时会失败。
<strong>apt-get update -qq &#x26;&#x26; apt-get install -yqq --no-install-recommends \
</strong>        curl \
        default-libmysqlclient-dev \
        libsasl2-dev \
        libsasl2-modules-gssapi-mit \
        libpq-dev \
        libecpg-dev \
        libldap2-dev 
        
apt-get install python3-dev  build-essential pkg-config 

<strong># 创建一个虚拟环境并激活
</strong>cd superset
python3 -m venv venv 
source venv/bin/activate

# 安装外部依赖
pip install -r requirements/development.txt

# 以可编辑（开发）模式安装 Superset
pip install -e .

#生成一个密匙，并加入到环境变量
echo 'export SUPERSET_SECRET_KEY="`(openssl rand -base64 42)`"' > \
        /etc/profile.d/superset_secret_key.sh

source /etc/profile

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
</code></pre>



或者使用Makefile安装

```bash
# Create a virtual environment and activate it (recommended)
python3 -m venv venv # setup a python3 virtualenv
source venv/bin/activate

# install pip packages + pre-commit
make install

# Install superset pip packages and setup env only
make superset
```



### 前端

#### 环境准备

首先，确保您使用的是以下版本的 Node.js 和 npm

* `Node.js`: Version 18
* `npm`: Version 10

建议使用 nvm 管理节点环境。使用nvm安装nvm与node：

<pre class="language-bash"><code class="lang-bash">curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash

#如果使用nvm命令出现 '-bash: nvm: command not found',请执行下面的命令或打开一个新的shell
export NVM_DIR="$HOME/.nvm"
<strong>[ -s "$NVM_DIR/nvm.sh" ] &#x26;&#x26; \. "$NVM_DIR/nvm.sh"  # This loads nvm
</strong>[ -s "$NVM_DIR/bash_completion" ] &#x26;&#x26; \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

cd superset-frontend
nvm install
nvm use
</code></pre>







#### 安装依赖模块

```bash
# 进入到superset-frontend目录
cd superset-frontend

# 安装来自 `package-lock.json`的依赖
npm ci
```

在执行npm run build构建前端资源时会消耗很大的内存，如果使用的是虚拟机的话建议将内存调高点，不然会一直卡住。

```bash
#构建前端资源
npm run build

#构建成功后启动前端
npm run dev-server
```

浏览器访问[http://localhost:9000](http://localhost:9000)

















