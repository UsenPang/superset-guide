# Docker代理配置

国内的docker镜像源被整改禁用了，如果你有梯子的话可以让docker使用梯子也是一种解决方案。

clash打开 Allow lan,这可以使你局域网中其他的电脑使用你的代理

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### Dockerd 代理

在执行docker pull时，是由守护进程dockerd来执行。因此，代理需要配在dockerd的环境中。而这个环境，则是受systemd所管控，因此实际是systemd的配置。

```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```

在这个proxy.conf文件（可以是任意\*.conf的形式）中，添加以下内容：

```
[Service]
Environment="HTTP_PROXY=http://proxy:7890/"
Environment="HTTPS_PROXY=http://proxy:7890/"
Environment="NO_PROXY=localhost,127.0.0.1,192.168.*"
```

配置文件中proxy需要修改为你上面开启局域网代理的ip。



重启docker服务

```
systemctl daemon-reload
systemctl restart docker
```

查看docker代理是否配置成功

```
docker info
```
