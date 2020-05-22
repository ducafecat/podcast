---
title: Docker - 01 windows 下安装 docker 并运行 yapi 服务
date: 2020-05-21 00:00:00
tags: Docker
categories: Docker
---

![](bg.jpg)

# 本节目标

- 安装 docker
- 启动 yapi
- 备份、恢复 yapi

## 正文

### 安装 Windows 10 专业工作站版

- i tell you

https://msdn.itellyou.cn/

- 选用 business 镜像

![](2020-05-18-14-40-53.png)

### 安装 docker

- 官网

https://www.docker.com/

![](2020-05-18-14-46-06.png)

- 启用 Hyper-V

![](2020-05-21-11-02-39.png)

- 切换 linunx container

![](2020-05-21-11-03-10.png)

### 阿里镜像加速

- 阿里镜像加速

https://cr.console.aliyun.com/cn-zhangjiakou/instances/mirrors

```sh
{
	"registry-mirrors": ["https://你的代码.mirror.aliyuncs.com"],
	"insecure-registries": [],
	"debug": true,
	"experimental": true
}
```

### docker-compose 配置 yapi

- .env

```
PASSWORD=$V7iTNk5N8#AkOeiwO@BywzBFte2^WsAuI$eJ4k9CKV0riqe
```

- docker-compose.yml

```yml
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
      # - ./docker-data/mongo-yapi:/data/db
      - mongo-data:/data/db
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

volumes:
  mongo-data:

networks:
  docker_net:
    driver: bridge
    ipam:
      config:
        - subnet: 172.22.0.0/16
```

### 启动、卸载 ypai 服务

- 启动

```sh
$ docker-compose up -d
```

- 卸载

```sh
$ docker-compose down
```

### 本地域名解析

- C:\Windows\System32\drivers\etc\hosts

```
127.0.0.1 api.news.ducafecat.tech
```

### 查询 volume

```sh
$ docker volume ls
DRIVER              VOLUME NAME
local               2fc91e2fd47a7110c2ecc5c8b88b997c4e6ddcf471a1df04f3fb618238ffd8aa
local               26e58cd678a97108f6dcd2cab33b9de341f992ceedacb7fd772c196bec908306
local               yapi-volumes_mongo-data
```

### 备份数据

```sh
$ docker run --rm --volumes-from mongo-ypai -v c:\backup:/backup ubuntu tar cvf /backup/backup.tar -C /data/db .
```

### 还原数据

```sh
$ docker run --rm --volumes-from mongo-ypai -v c:\backup:/backup ubuntu bash -c "cd /data/db && tar xvf /backup/backup.tar -C /data/db "
```

## 资源

### 参考

- https://github.com/fjc0k/docker-YApi
- https://cr.console.aliyun.com/cn-zhangjiakou/instances/mirrors
- https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes

### 视频

### 代码

https://github.com/ducafecat/docker-yapi.git

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
