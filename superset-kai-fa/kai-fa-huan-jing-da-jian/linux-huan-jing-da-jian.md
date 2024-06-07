# Linux 环境搭建

#### pkg-config报错

```bash
  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [24 lines of output]
      Trying pkg-config --exists mysqlclient
      Command 'pkg-config --exists mysqlclient' returned non-zero exit status 1.
      Trying pkg-config --exists mariadb
      Command 'pkg-config --exists mariadb' returned non-zero exit status 1.
      Trying pkg-config --exists libmariadb
      Command 'pkg-config --exists libmariadb' returned non-zero exit status 1.
      Traceback (most recent call last):
        File "/root/anaconda3/envs/zhaopin/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/root/anaconda3/envs/zhaopin/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
        File "/root/anaconda3/envs/zhaopin/lib/python3.10/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
        File "/tmp/pip-build-env-o5wmabdl/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
        File "/tmp/pip-build-env-o5wmabdl/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/tmp/pip-build-env-o5wmabdl/overlay/lib/python3.10/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 155, in <module>
        File "<string>", line 49, in get_config_posix
        File "<string>", line 28, in find_package_name
      Exception: Can not find valid pkg-config name.
      Specify MYSQLCLIENT_CFLAGS and MYSQLCLIENT_LDFLAGS env vars manually
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error
```

基本上就是缺失对应的依赖项，安装上就好了

**Debian / Ubuntu**

```
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential pkg-config 
```

**Red Hat / CentOS**

```
sudo yum install python3-devel mysql-devel pkgconfig 
```

详情请观看官网文档：

{% embed url="https://github.com/PyMySQL/mysqlclient#user-content-macos-homebrew" %}

#### 安装依赖python-ldap报错

错误信息：

```
 In file included from Modules/LDAPObject.c:8:0:
    Modules/constants.h:7:10: fatal error: lber.h: No such file or directory
     #include "lber.h"
              ^~~~~~~~
    compilation terminated.
    error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
```



解决：

&#x20;**Ubuntu 22.04**

```
sudo apt install libldap2-dev libsasl2-dev 
```

更详细的解决[https://stackoverflow.com/questions/56506294/installing-python-ldap-fails-with-lber-h-file-not-found-in-ubuntu-17-10-even-aft](https://stackoverflow.com/questions/56506294/installing-python-ldap-fails-with-lber-h-file-not-found-in-ubuntu-17-10-even-aft)
