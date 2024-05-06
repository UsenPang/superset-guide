# Ubuntu 22.0.4环境配置

#### 1、创建root用户

输入下面命令创建root用户，并设置密码

```bash
sudo passwd root
#切换到root用户
su root
```

#### 2、安装基本工具

```bash
apt install vim
apt install openssh-server
apt install net-tools
```

#### 3、配置ssh开机自启&#x20;

sudo systemctl enable ssh

#### 4、配置apt源为阿里源

```bash
sudo echo  \
"deb https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse

# deb https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
" > /etc/apt/sources.list

sudo apt update && apt upgrade
```

上面的命令会删除/ect/apt/sources.list 中的内容替换为命令中的内容。不同Ubuntu版本源的配置也不一样，不同版本的源可以参考[阿里镜像站](https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.330a1b11j9TGY3)



#### 5、安装Docker

增加阿里云Docker库

<pre class="language-bash"><code class="lang-bash"><strong>sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
</strong>
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
</code></pre>

安装Docker并检查是否安装成功

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl status docker
sudo systemctl start docker
sudo systemctl enable docker
sudo docker version
```

如果安装成功，docker version 会输出docker的版本信息



#### 6.配置固定ip（可选）

使用ifconfig会看到一串的信息，可以看到192.168.25.129为当前机器的IP地址，路由地址一般为该网络的第一个地址如：192.168.25.1，但是我使用的是VMware，路由为第二个地址192.168.25.2

```
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.25.129  netmask 255.255.255.0  broadcast 192.168.25.255
        inet6 fe80::20c:29ff:fe97:e9a0  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:97:e9:a0  txqueuelen 1000  (Ethernet)
        RX packets 925  bytes 103081 (103.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 520  bytes 119703 (119.7 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

配置固定ip会用到01-network-manager-all.yaml文件，主要修改的两个点：addresses（IP地址），routes （路由）

<pre class="language-bash"><code class="lang-bash">sudo echo \
"network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens33:	        # 配置的网卡名称，可以使用ifconfig -a查看本机的网卡
      dhcp4: no	#  关闭动态IP设置，因为要设置固定IP
      addresses: [<a data-footnote-ref href="#user-content-fn-1">192.168.25.129/24</a>]  # 要设置为的固定IP，后面的24为子网掩码的位数
      routes:	# 要设置的网关地址
        - to: default
          via: <a data-footnote-ref href="#user-content-fn-2">192.168.25.2 </a> 
      nameservers:
        addresses: [8.8.8.8,144.144.144.144]	# DNS服务器地址
" > /etc/netplan/01-network-manager-all.yaml
</code></pre>

使配置生效 `sudo netplan apply`

[^1]: 跟据自己所在网络IP配置

[^2]: 路由的地址
