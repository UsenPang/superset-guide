# Linux环境搭建

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



### 问题1

为什么使用docker compose启动登录后是一个空白页面？

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (17).png" alt="" width="375"><figcaption></figcaption></figure>

</div>

并且容器出现错误信息

Superset\_app:

```
2023-08-31 08:26:20,442:INFO:werkzeug:192.168.0.1 - - [31/Aug/2023 08:26:20] "GET /static/appbuilder/datepicker/bootstrap-datepicker.js HTTP/1.1" 200 -
2023-08-31 08:26:20,443:WARNING:superset.views.base:HTTPException
Traceback (most recent call last):
  File "/usr/local/lib/python3.9/site-packages/flask/app.py", line 1823, in full_dispatch_request
    rv = self.dispatch_request()
  File "/usr/local/lib/python3.9/site-packages/flask/app.py", line 1799, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)
  File "/usr/local/lib/python3.9/site-packages/flask/app.py", line 713, in <lambda>
    view_func=lambda **kw: self_ref().send_static_file(**kw),  # type: ignore # noqa: B950
  File "/usr/local/lib/python3.9/site-packages/flask/scaffold.py", line 331, in send_static_file
    return send_from_directory(
  File "/usr/local/lib/python3.9/site-packages/flask/helpers.py", line 590, in send_from_directory
    return werkzeug.utils.send_from_directory(  # type: ignore[return-value]
  File "/usr/local/lib/python3.9/site-packages/werkzeug/utils.py", line 578, in send_from_directory
    raise NotFound()
werkzeug.exceptions.NotFound: 404 Not Found: The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.
2023-08-31 08:26:20,457:INFO:werkzeug:192.168.0.1 - - [31/Aug/2023 08:26:20] "GET /static/appbuilder/js/ab.js HTTP/1.1" 200 -
2023-08-31 08:26:20,458:INFO:werkzeug:192.168.0.1 - - [31/Aug/2023 08:26:20] "GET /static/appbuilder/select2/select2.js HTTP/1.1" 200 -
2023-08-31 08:26:20,473:INFO:werkzeug:192.168.0.1 - - [31/Aug/2023 08:26:20] "GET /static/assets/images/superset-logo-horiz.png HTTP/1.1" 404 -
```

Superset\_node (Exit status 251):

```
npm ERR! code EIO
npm ERR! syscall rename
npm ERR! path /app/superset-frontend/node_modules/sha.js
npm ERR! dest /app/superset-frontend/node_modules/.sha.js-sPyHcmzN
npm ERR! errno -5
npm ERR! EIO: i/o error, rename '/app/superset-frontend/node_modules/sha.js' -> '/app/superset-frontend/node_modules/.sha.js-sPyHcmzN'

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2023-08-31T08_25_51_287Z-debug-0.log
```



1. superset\_app的报错指的是找不到静态资源
2. superset\_node的报错指的是该目录下已经有这个文件了，将会被重命名为.sha.js-sPyHcmzN

#### 如何解决？



其实最主要的问题还是出现在npm，npm在安装依赖是发现已经有这个文件，文件冲突了，导致前端的资源构建失败，出现问题1，然后登录之后返回空白的页面,。所以只需要将这些冲突的文件删掉就可以了，为了减少麻烦，在superset-frontend通过执行`find . -type d -name 'node_modules'`命令删除所有的node\_modules模块。docker compose重新运行。



### 问题2

superset\_init报错，并且登录页面登录不上去

```
The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/usr/local/bin/superset", line 8, in <module>
    sys.exit(superset())
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1157, in __call__
    return self.main(*args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1078, in main
    rv = self.invoke(ctx)
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1688, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1688, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1434, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 783, in invoke
    return __callback(*args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/click/decorators.py", line 33, in new_func
    return f(get_current_context(), *args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/flask/cli.py", line 358, in decorator
    return __ctx.invoke(f, *args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/click/core.py", line 783, in invoke
    return __callback(*args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/flask_migrate/cli.py", line 149, in upgrade
    _upgrade(directory, revision, sql, tag, x_arg)
  File "/usr/local/lib/python3.10/site-packages/flask_migrate/__init__.py", line 98, in wrapped
    f(*args, **kwargs)
  File "/usr/local/lib/python3.10/site-packages/flask_migrate/__init__.py", line 185, in upgrade
    command.upgrade(config, revision, sql=sql, tag=tag)
  File "/usr/local/lib/python3.10/site-packages/alembic/command.py", line 403, in upgrade
    script.run_env()
  File "/usr/local/lib/python3.10/site-packages/alembic/script/base.py", line 583, in run_env
    util.load_python_file(self.dir, "env.py")
  File "/usr/local/lib/python3.10/site-packages/alembic/util/pyfiles.py", line 95, in load_python_file
    module = load_module_py(module_id, path)
  File "/usr/local/lib/python3.10/site-packages/alembic/util/pyfiles.py", line 113, in load_module_py
    spec.loader.exec_module(module)  # type: ignore
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/app/superset/extensions/../migrations/env.py", line 127, in <module>
    run_migrations_online()
  File "/app/superset/extensions/../migrations/env.py", line 119, in run_migrations_online
    context.run_migrations()
  File "<string>", line 8, in run_migrations
  File "/usr/local/lib/python3.10/site-packages/alembic/runtime/environment.py", line 948, in run_migrations
    self.get_context().run_migrations(**kw)
  File "/usr/local/lib/python3.10/site-packages/alembic/runtime/migration.py", line 627, in run_migrations
    step.migration_fn(**kw)
  File "/app/superset/migrations/versions/2023-09-15_12-58_4b85906e5b91_add_on_delete_cascade_for_dashboard_roles.py", line 50, in upgrade
    redefine(foreign_key, on_delete="CASCADE")
  File "/app/superset/migrations/shared/constraints.py", line 57, in redefine
    with op.batch_alter_table(foreign_key.table, naming_convention=conv) as batch_op:
  File "/usr/local/lib/python3.10/contextlib.py", line 142, in __exit__
    next(self.gen)
  File "/usr/local/lib/python3.10/site-packages/alembic/operations/base.py", line 398, in batch_alter_table
    impl.flush()
  File "/usr/local/lib/python3.10/site-packages/alembic/operations/batch.py", line 116, in flush
    fn(*arg, **kw)
  File "/usr/local/lib/python3.10/site-packages/alembic/ddl/impl.py", line 350, in drop_constraint
    self._exec(schema.DropConstraint(const))
  File "/usr/local/lib/python3.10/site-packages/alembic/ddl/impl.py", line 207, in _exec
    return conn.execute(construct, multiparams)
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/base.py", line 1385, in execute
    return meth(self, multiparams, params, _EMPTY_EXECUTION_OPTS)
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/sql/ddl.py", line 80, in _execute_on_connection
    return connection._execute_ddl(
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/base.py", line 1477, in _execute_ddl
    ret = self._execute_context(
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/base.py", line 1953, in _execute_context
    self._handle_dbapi_exception(
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/base.py", line 2134, in _handle_dbapi_exception
    util.raise_(
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/util/compat.py", line 211, in raise_
    raise exception
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/base.py", line 1910, in _execute_context
    self.dialect.do_execute(
  File "/usr/local/lib/python3.10/site-packages/sqlalchemy/engine/default.py", line 736, in do_execute
    cursor.execute(statement, parameters)
sqlalchemy.exc.OperationalError: (psycopg2.errors.DeadlockDetected) deadlock detected
DETAIL:  Process 77 waits for AccessExclusiveLock on relation 16405 of database 16384; blocked by process 79.
Process 79 waits for AccessShareLock on relation 16413 of database 16384; blocked by process 77.
HINT:  See server log for query details.

[SQL: ALTER TABLE dashboard_roles DROP CONSTRAINT dashboard_roles_role_id_fkey]
(Background on this error at: https://sqlalche.me/e/14/e3q8)

```



解决这个问题也很简单，使用`docker compose restart superset-init` 重启一下superset\_init就好了
