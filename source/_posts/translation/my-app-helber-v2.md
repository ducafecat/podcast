---
title: Flutter 到底能不能做 APP， GetX 能实战么，我上架了一款APP Helber
tags: flutter
categories: Flutter见闻
date: 2021-10-26 00:00:00
---

![](2021-10-26-11-26-09.png)

## 前言

群里有不少新加入的朋友，大家会有一个疑惑，就是 Flutter 做 app 到底靠谱么。

还有这个 GetX 实战中的表现如何，是否有大坑。

我这边上架了一款产品 helber

- 官方
  https://helberapp.com/

- 苹果店
  https://apps.apple.com/app/id1533390110

- 谷歌店
  https://play.google.com/store/apps/details?id=com.zykj.qubang

![](2021-10-26-09-33-32.png)

### 用到的组件

```yaml
# The following adds the Cupertino Icons font to your application.
# Use with the CupertinoIcons class for iOS style icons.
cupertino_icons: ^1.0.2
get: ^4.3.6
dio: ^4.0.0
# 权限
permission_handler: ^8.1.2
# app信息
package_info: ^2.0.2
# 本地存储
shared_preferences: ^2.0.8
# 刷新加载
pull_to_refresh: ^2.0.0
# toast 提示
flutter_easyloading: ^3.0.3
# 底部弹出框
modal_bottom_sheet: ^2.0.0
# 输入框
pinput: ^1.2.0
# 适配屏幕
flutter_screenutil: ^5.0.0+2
# 网络图片
cached_network_image: ^3.1.0
# 媒体选择
wechat_assets_picker: ^6.0.4
wechat_camera_picker: ^2.4.1
# 滑块
carousel_slider: ^4.0.0
# svg
flutter_svg: ^0.22.0
# 瀑布流
waterfall_flow: ^3.0.1
# 加密
crypto: ^3.0.1
# OSS
aliyun_oss_flutter: ^1.0.5
# 视频图片压缩
video_compress: ^3.1.0
flutter_image_compress: ^1.1.0
# 图片预览
photo_view: ^0.12.0
# 视频播放
chewie: ^1.2.2
video_player: ^2.2.5
# 选择
# flutter_cupertino_datetime_picker: ^2.0.1
flutter_picker: ^2.0.2
# 时间转换
intl: ^0.17.0
# 定位
geolocator: ^7.6.2
# 地图
google_maps_flutter: ^2.0.11
google_maps_cluster_manager: ^3.0.0+1
# 缓存
flutter_cache_manager: ^3.1.2
# webkit
webview_flutter: ^2.0.12
# 打开url
url_launcher: ^6.0.12
# 升级
r_upgrade: ^0.3.5
version: ^2.0.0
# app 打开 uri
uni_links: ^0.5.1
# IM
tencent_im_sdk_plugin: ^3.5.0
# 腾讯推送
tpns_flutter_plugin:
  git:
    url: https://gitee.com/ducafecat/TPNS-Flutter-Plugin
# google sign
google_sign_in: ^5.1.1
# apple sign
sign_in_with_apple: ^3.2.0
# facebook sign
flutter_facebook_auth: ^3.5.2
# sentry
sentry_flutter: ^6.0.1
# 头部背景
# draggable_home: ^1.0.2
# 第三方登录按钮
auth_buttons: ^1.0.1+4
# 倒计时
timer_count_down: ^2.2.0
```

### 项目规模

页数: 40~50

![](2021-10-26-09-22-52.png)

### 业务

- 社交信息
- 积分系统
- 商品兑换
- 商家端

### 技术点

- 地理定位
- 长列表
- 拍照、拍视频
- 阿里 oss
- 图片缓存
- 图片预览
- 缩率图
- 腾讯聊天
- 腾讯消息推送 TPNS
- pin 安全
- 数据离线
- 三方登录 谷歌、苹果、facebook

### 性能测试

- 帧率

![](2021-10-26-09-50-23.png)

- 性能图层

![](2021-10-26-09-51-19.png)

- CPU

![](2021-10-26-09-54-12.png)

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

![](https://ducafecat.tech/img/public-qrcode.png)

## 往期

### 开源

#### GetX Quick Start

https://github.com/ducafecat/getx_quick_start

#### 新闻客户端

https://github.com/ducafecat/flutter_learn_news

### strapi 手册译文

https://getstrapi.cn

### 微信讨论群 ducafecat

### 系列集合

#### 译文

https://ducafecat.tech/categories/%E8%AF%91%E6%96%87/

#### 开源项目

https://ducafecat.tech/categories/%E5%BC%80%E6%BA%90/

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
