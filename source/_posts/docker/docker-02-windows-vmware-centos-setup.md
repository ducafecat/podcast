---
title: Docker - 02 前端全栈 windows 从零安装 centos 7 docker 的正确姿势
date: 2020-05-29 00:00:00
tags: Docker Linux
categories: Docker
---

![](bg.jpg)

# 本节目标

- 前端全栈主力操作系统选哪个 ?
- windows 下使用 docker 为什么不行 ?
- VMWare 安装 centos
- 远程 centos 系统
- centos 配置 docker环境

## 视频

## 正文

### 1. 主力操作系统分析

从前端全栈角度考虑

|                                      | windows | macos | ubuntu |
| ------------------------------------ | ------- | ----- | ------ |
| nodejs、java、go、python             | ok      | ok    | ok     |
| vue、react、electron、rn             | ok      | ok    | ok     |
| 小程序                               | ok      | ok    | ok     |
| ios                                  |         | ok    |        |
| android                              | ok      | ok    | ok     |
| 办公 office wps ps 微信 QQ XD VSCode | ok      | ok    | ok     |
| macos 专属 safri sketch              |         | ok    |        |
| 程序编译、文件名大小写严格           |         | ok    | ok     |



### 2. windows 直接用 docker 存在的问题

**问题：**

https://docs.docker.com/get-started/overview/
https://docs.docker.com/get-started/
https://docs.microsoft.com/en-us/windows/wsl/about

- 容器架构不同

- 切到 linux 容器架构，频繁遇到存储驱动兼容问题

- WSL 2 的路还很长

**总结**

开发环境与线上环境不一致，引发不必要的联调成本。

### 3. windows 下 vmware 安装 centos

#### 3.1 下载 centos 7

http://isoredirect.centos.org/centos/7/isos/x86_64/

#### 3.2 安装 centos 7

操作见视频

#### 3.3 配置 centos 网卡

操作见视频

指令记录

```shell
# 修改配置
$ cd /etc/sysconfig/network-scripts
$ ll
$ vi ifcfg-eth0
ONBOOT=yes

# vi 文件编辑
# cd 进入目录
# ll 目录列表

------------------------------

# 重启网卡
$ service network restart

# service 管理系统服务

------------------------------

# 安装工具
$ yum install -y net-tools

# yum 软件包管理

------------------------------

# 查看ip
$ ifconfig

------------------------------

# 固定ip、dns
$ vi ifcfg-eth0
BOOTPROTO=static
IPADDR=10.211.55.5
NETMASK=255.255.255.0
GATEWAY=10.211.55.1
DNS1=223.5.5.5
DNS2=223.6.6.6

------------------------------

# 重启服务、查看dns
$ service network restart
$ cat /etc/resolv.conf
```



#### 3.4 远程 ssh 工具

- finalshell
  http://www.hostbuf.com

- xshell、sftp
  https://www.netsarang.com/zh/xshell/

- putty
  https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

- cmder
  http://cmder.net/

  

#### 3.6 安装 docker

##### 卸载旧版

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

> sudo root 用户可以不用

##### 原生安装

```shell
# 系统工具
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 加仓库
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 安装 docker ce cli
$ sudo yum install -y docker-ce docker-ce-cli containerd.io

# 启动服务
$ sudo systemctl start docker

# 开机启动
$ sudo systemctl enable docker

# 安装 docker-compose
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 阿里云加速
$ sudo mkdir -p /etc/docker
$ sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://8stycbeq.mirror.aliyuncs.com"]
}
EOF
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

### 4. 运行 yapi

#### 4.1 docker-compose 配置

这次修改了数据持久化在指定目录

```yaml
version: "3"
services:
  mongo-yapi:
    image: mongo
    container_name: mongo-ypai
    restart: always
    # ports:
    #   - 27017:27017
    environment:
      - TZ=Asia/Shanghai
      - MONGO_INITDB_DATABASE=yapi
      # - MONGO_INITDB_ROOT_USERNAME=root
      # - MONGO_INITDB_ROOT_PASSWORD=${PASSWORD}
    volumes:
      - ./docker-data/mongo-yapi:/data/db
    networks:
      docker_net:
        ipv4_address: 172.22.0.11

  # https://github.com/fjc0k/docker-YApi
  web-yapi:
    image: jayfong/yapi:latest
    container_name: web-ypai
    restart: always
    ports:
      - 3000:3000
    depends_on:
      - mongo-yapi
    links:
      - mongo-yapi
    environment:
      - TZ=Asia/Shanghai
      - YAPI_ADMIN_ACCOUNT=admin@ducafecat.tech
      - YAPI_ADMIN_PASSWORD=${PASSWORD}
      - YAPI_CLOSE_REGISTER=true
      - YAPI_DB_SERVERNAME=mongo-yapi
      - YAPI_DB_PORT=27017
      - YAPI_DB_DATABASE=yapi
      # - YAPI_DB_USER=root
      # - YAPI_DB_PASS=${PASSWORD}
      - YAPI_MAIL_ENABLE=false
      - YAPI_LDAP_LOGIN_ENABLE=false
      - YAPI_PLUGINS=[]
    networks:
      docker_net:
        ipv4_address: 172.22.0.12

networks:
  docker_net:
    driver: bridge
    ipam:
      config:
        - subnet: 172.22.0.0/16

```

#### 4.2 运行 yapi

```shell
# 启动
$ docker-compose up -d

# 卸载
$ docker-compose down
```



#### 4.3 修改本地解析

C:\Windows\System32\drivers\etc\hosts

```shell
127.0.0.1 api.news.ducafecat.tech
```



## 问题整理

### VMware Workstation 与 Device/Credential Guard 不兼容。

网上说卸载 Hyper-V， 没必要卸载

管理员方式运行 cmd 执行

```shell
bcdedit /set hypervisorlaunchtype off
```

然后重启电脑




## 参考

- install docker

  https://docs.docker.com/engine/install/centos/ 

- install docker-compose

  https://github.com/docker/compose/releases 

- 阿里镜像

  https://cr.console.aliyun.com/cn-beijing/instances/mirrors





---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
