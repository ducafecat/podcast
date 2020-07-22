---
title: Flutter 实战从零开始 新闻客户端 - 17 strapi centos 发布部署 + jmeter 压测
date: 2020-07-21 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](2020-07-22-14-40-04.png)

# 本节目标

- 上传代码到生产环境
- 配置发布环境代码
- docker-compose 方式启动项目
- jmeter 做基线测试 调优服务器配置

## 视频

## strapi 运行环境网盘下载

- 网盘

链接:https://pan.baidu.com/s/13Ujy2hzXp8tSqxCx_4IhVQ
密码:yu82

- 运行

本节下载 strapi-docker-compose-16.zip 这个文件

需要用 docker-compose 启动
账号 admin
密码 123456

```sh
# 启动
docker-compose up -d --remove-orphans

# 关闭
docker-compose down
```

## 正文

### 打包上传服务器

- 服务器配置

考虑到基线压测优化，所以一开始低些

服务器 cpu1 核心 2G 内存

- 如何配置 centos 服务器

参考我之前的文章

https://ducafecat.tech/2020/05/29/docker/docker-02-windows-vmware-centos-setup/

https://youtu.be/NJIwbs8qmDY

- 把所有文件 tar、zip 打包后上传

```sh
> yum install -y zip unzip

> unzip filename.zip
```

### 修改 graphql 配置

- 文件 ./config/plugins.js

```js
module.exports = {
  //
  graphql: {
    endpoint: "/graphql",
    tracing: false,
    shadowCRUD: true,
    playgroundAlways: false,
    depthLimit: 7,
    amountLimit: 100,
  },
};
```

| 名称             | 说明           |
| ---------------- | -------------- |
| endpoint         | 对外路径       |
| tracing          | 反馈性能报告   |
| shadowCRUD       | 支持 CRUD 操作 |
| playgroundAlways | 显示调试界面   |
| depthLimit       | 查询深度限制   |
| amountLimit      | 查询项目个数   |

- 参数说明

### 修改 admin 面板地址

- 文件 ./config/server.js

```js
module.exports = ({ env }) => ({
  host: env("HOST", "0.0.0.0"),
  port: env.int("PORT", 1337),
  admin: {
    url: "/dashboard",
  },
});
```

> 默认 admin 不安全，可以改成奇怪的路径 dfasdx97s7 这种

### strapi build 编译

- 安装 portainer

```sh
docker run -d -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --name portainer-local \
    portainer/portainer
```

- 用工具 portainer 进入容器

![](2020-07-22-09-23-25.png)

![](2020-07-22-09-36-35.png)

```sh
> strapi build
```

### 修改 docker-compose 命令

```yaml
  ...
  strapi-app:
    image: strapi/strapi
    container_name: strapi-app
    restart: always
    ports:
      - 1337:1337
    command: strapi start
    environment:
      ...
```

### jmeter 压第一轮 - 服务器 cpu1 核心 2G 内存

- 测试工具

https://jmeter.apache.org/download_jmeter.cgi

> 需要安装 java 运行环境 jre 1.8

- 测试内容

新闻首页

```
post /graphql

{"operationName":"pageIndex","variables":{},"query":"query pageIndex {\n  dictCategories(sort: \"sortNum:desc\") {\n    code\n    title\n  }\n  dictChannels(sort: \"sortNum:desc\") {\n    code\n    title\n    icon {\n      url\n    }\n  }\n  busNews(where: {dict_categories: {code: \"news_hot\"}}) {\n    title\n    dict_channel {\n      code\n      title\n      icon {\n        url\n      }\n    }\n    dict_categories {\n      code\n      title\n    }\n    author\n    url\n    addtime\n    thumbnail {\n      url\n    }\n  }\n}\n"}
```

![](2020-07-22-09-29-05.png)

![](2020-07-22-09-29-43.png)

- 测试环境

压测机: 8 核心 32G 本机 macos，发起线程数 1000~2000 官方建议单机数量
服务器: 1 核心 2G 虚拟机 centos

- 服务器信息监控

```sh
> top

top - 23:41:47 up 45 min,  2 users,  load average: 0.12, 2.10, 2.09
Tasks: 160 total,   1 running, 159 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.2 us,  1.2 sy,  0.0 ni, 96.3 id,  0.0 wa,  0.0 hi,  0.2 si,  0.0 st
KiB Mem :  1877360 total,   424344 free,   746224 used,   706792 buff/cache
KiB Swap:  6713340 total,  6713340 free,        0 used.   963492 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
31814 root      20   0 2099204  91620  23620 S   3.7  4.9   3:03.04 node
  977 root      20   0  820140  47944  14876 S   2.0  2.6   0:40.63 containerd
    9 root      20   0       0      0      0 S   0.3  0.0   0:01.84 rcu_sched
  445 root      20   0       0      0      0 S   0.3  0.0   0:01.07 xfsaild/dm-0
  995 root      20   0  783740  91376  29532 S   0.3  4.9   0:08.46 dockerd
 1675 polkitd   20   0 1659672 119912  18932 S   0.3  6.4   2:15.62 mongod
31570 root      20   0  633704  38776  15156 S   0.3  2.1   0:13.24 node
    1 root      20   0  191036   3940   2572 S   0.0  0.2   0:00.60 systemd
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd
    4 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
    6 root      20   0       0      0      0 S   0.0  0.0   0:01.15 ksoftirqd/0
    7 root      rt   0       0      0      0 S   0.0  0.0   0:00.09 migration/0
    8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
```

> ctrl + c 停止

- GUI 压测

> 1000 个请求 10 秒内

![](2020-07-22-10-46-12.png)

![](2020-07-22-10-49-10.png)

- 命令行 压测

```sh
jmeter -n -t strapi.jmx -l report/result.csv -j report/log.log -e -o report_html
```

![](2020-07-22-10-53-49.png)

### jmeter 压第二轮 - 服务器 cpu4 核心 2G 内存

- 测试环境

压测机: 8 核心 32G 本机 macos，发起线程数 1000~2000
服务器: 4 核心 2G 虚拟机 centos

![](2020-07-22-11-11-19.png)

> 1000 个请求 5 秒内

- 压测报告

```sh
jmeter -n -t strapi.jmx -l report/result.csv -j report/log.log -e -o report_html
```

![](2020-07-22-11-12-33.png)

### strapi 优化线程数

- 安装 pm2 工具

https://pm2.keymetrics.io/docs/usage/quick-start/

- 编写 启动代码 server.js

```js
const strapi = require("strapi");
strapi().start();
```

- 编写 配置文件 strapi.config.js

```js
module.exports = {
  apps: [
    {
      name: "strapi-app",
      script: "./server.js",
      instances: 4,
      exec_mode: "cluster",
    },
  ],
};
```

> instances 等于你的核心数

- 修改 docker-compose.yml

```yaml
  ...
strapi-app:
  image: strapi/strapi
  container_name: strapi-app
  restart: always
  ports:
    - 1337:1337
  # command: strapi build
  # command: strapi start
  command:
    - /bin/bash
    - -c
    - |
      npm install pm2@latest -g
      cd /srv/app
      pm2-runtime start strapi.config.js
  environment:
    ...
```

### jmeter 压第三轮 - 服务器 cpu4 核心 2G 内存

![](2020-07-22-11-37-44.png)

> 2000 个请求 1 秒内

![](2020-07-22-11-38-41.png)

### 主要服务器指标

| 名称 | 说明   |
| ---- | ------ |
| CPU  | <= 40% |
| 内存 | <= 70% |

> 其它主要指标 连接数、本机进出流量、内网带宽、外网带宽

## 总结

- strapi 服务器部署配置
- jmetre 基线测试方法、主要指标查看

## 参考

- strapi 插件配置

https://strapi.io/documentation/v3.x/plugins/graphql.html

- strapi 命令行

https://strapi.io/documentation/v3.x/cli/CLI.html#strapi-new

- strapi 部署说明

https://strapi.io/documentation/v3.x/admin-panel/deploy.html

- pm2 安装

https://pm2.keymetrics.io/docs/usage/quick-start/

- pm2 配置文件

https://pm2.keymetrics.io/docs/usage/application-declaration/

- jmeter

https://jmeter.apache.org/download_jmeter.cgi

- jmeter 分布式

https://jmeter.apache.org/usermanual/jmeter_distributed_testing_step_by_step.html

## 资源

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
