---
title: Flutter 实战从零开始 新闻客户端 - 14 headless strapi + graphql 快速构建新闻后台
date: 2020-07-03 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](2020-07-03-17-43-32.png)

# 本节目标

- strapi + graphql 插件 + docker 安装
- strapi 管理数据结构、内容
- flutter + graphql 插件 实现查询

## 视频

<iframe src="//player.bilibili.com/player.html?bvid=BV1Zz411e7dj&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.14

## 正文

### 后台开发步骤

![](2020-07-04-13-47-38.png)

### 采用 strapi + nodejs + 网关 的方案

![](2020-07-04-13-48-08.png)

### 1. strapi 安装

#### 1.1 docker-compose 方式安装

- .env

```
PASSWORD=123456
```

- docker-compose.yml

```yaml
version: "3"
services:
  mongo:
    image: mongo
    container_name: mongo
    restart: always
    ports:
      - 27017:27017
    environment:
      - TZ=Asia/Shanghai
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=${PASSWORD}
    volumes:
      - ./docker-data/mongo:/data/db
    networks:
      docker_net:
        ipv4_address: 172.22.0.11

  # starpi
  # admin / 123456 / admin@ducafecat.tech
  strapi-app:
    image: strapi/strapi
    container_name: strapi-app
    restart: always
    ports:
      - 1337:1337
    # command: strapi build
    # command: strapi start
    environment:
      - TZ=Asia/Shanghai
      - DATABASE_CLIENT=mongo
      - DATABASE_HOST=mongo
      - DATABASE_PORT=27017
      - DATABASE_NAME=strapi
      - DATABASE_USERNAME=root
      - DATABASE_PASSWORD=${PASSWORD}
      - DATABASE_AUTHENTICATION_DATABASE=strapi
      # - NODE_ENV=production
    depends_on:
      - mongo
    volumes:
      - ./docker-data/strapi-app:/srv/app
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

http://localhost:1337/admin

#### 1.2 安装 graphql 插件

![](2020-07-03-16-45-52.png)

### 2. 构建新闻数据结构

#### 2.1 创建数据类型

- 添加类型

![](2020-07-03-16-47-15.png)

- 添加字段

![](2020-07-03-16-47-35.png)

- 字段列表

![](2020-07-03-16-46-25.png)

#### 2.2 调整数据编辑界面

![](2020-07-03-16-48-29.png)

#### 2.3 调整数据列表界面

![](2020-07-03-16-48-55.png)

#### 2.4 维护数据

- 列表

![](2020-07-03-16-49-37.png)

- 添加

![](2020-07-03-16-49-59.png)

### 3. 调试 graphql 请求

#### 3.3 graphql 语法

- 类型
  - query 查询
  - mutate 操作

#### 3.4 调试新闻列表

http://localhost:1337/graphql

![](2020-07-03-16-50-31.png)

### 4. 编写 flutter 代码

#### 4.1 加入 graphql 插件

https://pub.flutter-io.cn/packages/graphql

- pubspec.yaml

```yaml
dependencies:
  # graphql
  graphql: ^3.0.2
```

#### 4.2 封装 graphql client 工具类

- lib/common/utils/graphql_client.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/values/values.dart';
import 'package:flutter_ducafecat_news/common/widgets/widgets.dart';
import 'package:graphql/client.dart';

class GraphqlClientUtil {
  static OptimisticCache cache = OptimisticCache(
    dataIdFromObject: typenameDataIdFromObject,
  );

  static client() {
    HttpLink _httpLink = HttpLink(
      uri: '$SERVER_STRAPI_GRAPHQL_URL/graphql',
    );

    // final AuthLink _authLink = AuthLink(
    //   getToken: () =>
    //       'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjVlZmMzNDdhYzgzOTVjMDAwY2ViYzE5NyIsImlhdCI6MTU5MzY1NDcwNiwiZXhwIjoxNTk2MjQ2NzA2fQ.RYDmNSDJxcZLLPHAf4u59IER7Bs5VoWfBo1_t-TR5yY',
    // );

    // final Link _link = _authLink.concat(_httpLink);

    return GraphQLClient(
      cache: cache,
      link: _httpLink,
    );
  }

  // 查询
  static Future query({
    @required BuildContext context,
    @required String schema,
    Map<String, dynamic> variables,
  }) async {
    QueryOptions options = QueryOptions(
      documentNode: gql(schema),
      variables: variables,
    );

    QueryResult result = await client().query(options);

    if (result.hasException) {
      toastInfo(msg: result.exception.toString());
      throw result.exception;
    }

    return result;
  }

  // 操作
  static Future mutate({
    @required BuildContext context,
    @required String schema,
    Map<String, dynamic> variables,
  }) async {
    QueryOptions options = QueryOptions(
      documentNode: gql(schema),
      variables: variables,
    );

    QueryResult result = await client().mutate(options);

    if (result.hasException) {
      toastInfo(msg: result.exception.toString());
      throw result.exception;
    }

    return result;
  }
}

```

#### 4.3 编写 graphql 查询请求

- lib/common/graphql/news_content.dart

```dart
const String GQL_NEWS_LIST = r'''
  query News {
    newsContents {
      title
      category
      author
      url
      addtime
      thumbnail {
        url
      }
    }
  }
''';
```

#### 4.4 编写数据实体

lib/common/entitys/gql_news.dart

```dart
class GqlNewsResponseEntity {
  GqlNewsResponseEntity({
    this.id,
    this.title,
    this.category,
    this.author,
    this.url,
    this.addtime,
    this.thumbnail,
  });

  String id;
  String title;
  String category;
  String author;
  String url;
  DateTime addtime;
  Thumbnail thumbnail;

  factory GqlNewsResponseEntity.fromJson(Map<String, dynamic> json) =>
      GqlNewsResponseEntity(
        id: json["id"],
        title: json["title"],
        category: json["category"],
        author: json["author"],
        url: json["url"],
        addtime: DateTime.parse(json["addtime"]),
        thumbnail: Thumbnail.fromJson(json["thumbnail"]),
      );

  Map<String, dynamic> toJson() => {
        "id": id,
        "title": title,
        "category": category,
        "author": author,
        "url": url,
        "addtime":
            "${addtime.year.toString().padLeft(4, '0')}-${addtime.month.toString().padLeft(2, '0')}-${addtime.day.toString().padLeft(2, '0')}",
        "thumbnail": thumbnail.toJson(),
      };
}

class Thumbnail {
  Thumbnail({
    this.url,
  });

  String url;

  factory Thumbnail.fromJson(Map<String, dynamic> json) => Thumbnail(
        url: json["url"],
      );

  Map<String, dynamic> toJson() => {
        "url": url,
      };
}

```

#### 4.5 编写 API 访问

- lib/common/apis/gql_news.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/graphql/graphql.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';
import 'package:graphql/client.dart';

/// 新闻
class GqlNewsAPI {
  /// 翻页
  static Future<List<GqlNewsResponseEntity>> newsPageList({
    @required BuildContext context,
    Map<String, dynamic> params,
  }) async {
    QueryResult response =
        await GraphqlClientUtil.query(context: context, schema: GQL_NEWS_LIST);

    return response.data['newsContents']
        .map<GqlNewsResponseEntity>(
            (item) => GqlNewsResponseEntity.fromJson(item))
        .toList();
  }
}

```

#### 4.6 修改新闻列表页

- lib/pages/main/main.dart

```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/apis/apis.dart';
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';
import 'package:flutter_ducafecat_news/common/values/values.dart';
import 'package:flutter_ducafecat_news/common/widgets/widgets.dart';
import 'package:flutter_ducafecat_news/pages/main/ad_widget.dart';
import 'package:flutter_ducafecat_news/pages/main/categories_widget.dart';
import 'package:flutter_ducafecat_news/pages/main/channels_widget.dart';
import 'package:flutter_ducafecat_news/pages/main/news_item_widget.dart';
import 'package:flutter_ducafecat_news/pages/main/newsletter_widget.dart';
import 'package:flutter_ducafecat_news/pages/main/recommend_widget.dart';
import 'package:flutter_easyrefresh/easy_refresh.dart';

class MainPage extends StatefulWidget {
  MainPage({Key key}) : super(key: key);

  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  EasyRefreshController _controller; // EasyRefresh控制器

  // NewsPageListResponseEntity _newsPageList; // 新闻翻页
  List<GqlNewsResponseEntity> _newsPageList; // 新闻翻页

  NewsItem _newsRecommend; // 新闻推荐
  List<CategoryResponseEntity> _categories; // 分类
  List<ChannelResponseEntity> _channels; // 频道

  String _selCategoryCode; // 选中的分类Code

  @override
  void initState() {
    super.initState();
    _controller = EasyRefreshController();
    _loadAllData();
    _loadLatestWithDiskCache();
  }

  // 如果有磁盘缓存，延迟3秒拉取更新档案
  _loadLatestWithDiskCache() {
    if (CACHE_ENABLE == true) {
      var cacheData = StorageUtil().getJSON(STORAGE_INDEX_NEWS_CACHE_KEY);
      if (cacheData != null) {
        Timer(Duration(seconds: 3), () {
          _controller.callRefresh();
        });
      }
    }
  }

  // 读取所有数据
  _loadAllData() async {
    _categories = await NewsAPI.categories(
      context: context,
      cacheDisk: true,
    );
    _channels = await NewsAPI.channels(
      context: context,
      cacheDisk: true,
    );
    // _newsRecommend = await NewsAPI.newsRecommend(
    //   context: context,
    //   cacheDisk: true,
    // );

    // _newsPageList = await NewsAPI.newsPageList(
    //   context: context,
    //   cacheDisk: true,
    // );
    _newsPageList = await GqlNewsAPI.newsPageList(
      context: context,
    );

    _selCategoryCode = _categories.first.code;

    if (mounted) {
      setState(() {});
    }
  }

  // 拉取推荐、新闻
  _loadNewsData(
    categoryCode, {
    bool refresh = false,
  }) async {
    _selCategoryCode = categoryCode;
    _newsRecommend = await NewsAPI.newsRecommend(
      context: context,
      params: NewsRecommendRequestEntity(categoryCode: categoryCode),
      refresh: refresh,
      cacheDisk: true,
    );
    // _newsPageList = await NewsAPI.newsPageList(
    //   context: context,
    //   params: NewsPageListRequestEntity(categoryCode: categoryCode),
    //   refresh: refresh,
    //   cacheDisk: true,
    // );
    _newsPageList = await GqlNewsAPI.newsPageList(
      context: context,
    );

    if (mounted) {
      setState(() {});
    }
  }

  // 分类菜单
  Widget _buildCategories() {
    return _categories == null
        ? Container()
        : newsCategoriesWidget(
            categories: _categories,
            selCategoryCode: _selCategoryCode,
            onTap: (CategoryResponseEntity item) {
              _loadNewsData(item.code);
            },
          );
  }

  // 推荐阅读
  Widget _buildRecommend() {
    return _newsRecommend == null // 数据没到位，可以用骨架图展示
        ? Container()
        : recommendWidget(_newsRecommend);
  }

  // 频道
  Widget _buildChannels() {
    return _channels == null
        ? Container()
        : newsChannelsWidget(
            channels: _channels,
            onTap: (ChannelResponseEntity item) {},
          );
  }

  // 新闻列表
  Widget _buildNewsList() {
    return _newsPageList == null
        ? Container(
            height: duSetHeight(161 * 5 + 100.0),
          )
        : Column(
            children: _newsPageList.map((item) {
              // 新闻行
              List<Widget> widgets = <Widget>[
                newsItem(item),
                Divider(height: 1),
              ];

              // 每 5 条 显示广告
              int index = _newsPageList.indexOf(item);
              if (((index + 1) % 5) == 0) {
                widgets.addAll(<Widget>[
                  adWidget(),
                  Divider(height: 1),
                ]);
              }

              // 返回
              return Column(
                children: widgets,
              );
            }).toList(),
          );
  }

  // ad 广告条
  // 邮件订阅
  Widget _buildEmailSubscribe() {
    return newsletterWidget();
  }

  @override
  Widget build(BuildContext context) {
    return _newsPageList == null
        ? cardListSkeleton()
        : EasyRefresh(
            enableControlFinishRefresh: true,
            controller: _controller,
            header: ClassicalHeader(),
            onRefresh: () async {
              await _loadNewsData(
                _selCategoryCode,
                refresh: true,
              );
              _controller.finishRefresh();
            },
            child: SingleChildScrollView(
              child: Column(
                children: <Widget>[
                  _buildCategories(),
                  Divider(height: 1),
                  _buildRecommend(),
                  Divider(height: 1),
                  _buildChannels(),
                  Divider(height: 1),
                  _buildNewsList(),
                  Divider(height: 1),
                  _buildEmailSubscribe(),
                ],
              ),
            ),
          );
  }
}

```

#### 4.7 修改新闻详情页

- lib/pages/main/news_item_widget.dart

```dart
import 'package:auto_route/auto_route.dart';
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';
import 'package:flutter_ducafecat_news/common/values/values.dart';
import 'package:flutter_ducafecat_news/common/widgets/widgets.dart';
import 'package:flutter_ducafecat_news/common/router/router.gr.dart';

/// 新闻行 Item
Widget newsItem(GqlNewsResponseEntity item) {
  return Container(
    height: duSetHeight(161),
    padding: EdgeInsets.all(duSetWidth(20)),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        // 图
        InkWell(
          onTap: () {
            ExtendedNavigator.rootNavigator.pushNamed(
              Routes.detailsPageRoute,
              arguments: DetailsPageArguments(item: item),
            );
          },
          child: imageCached(
            '$SERVER_STRAPI_GRAPHQL_URL${item.thumbnail.url}',
            width: duSetWidth(121),
            height: duSetWidth(121),
          ),
        ),
        // 右侧
        SizedBox(
          width: duSetWidth(194),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              // 作者
              Container(
                margin: EdgeInsets.all(0),
                child: Text(
                  item.author,
                  style: TextStyle(
                    fontFamily: 'Avenir',
                    fontWeight: FontWeight.normal,
                    color: AppColors.thirdElementText,
                    fontSize: duSetFontSize(14),
                    height: 1,
                  ),
                ),
              ),
              // 标题
              InkWell(
                onTap: () {
                  ExtendedNavigator.rootNavigator.pushNamed(
                    Routes.detailsPageRoute,
                    arguments: DetailsPageArguments(item: item),
                  );
                },
                child: Container(
                  margin: EdgeInsets.only(top: duSetHeight(10)),
                  child: Text(
                    item.title,
                    style: TextStyle(
                      fontFamily: 'Montserrat',
                      fontWeight: FontWeight.w500,
                      color: AppColors.primaryText,
                      fontSize: duSetFontSize(16),
                      height: 1,
                    ),
                    overflow: TextOverflow.clip,
                    maxLines: 3,
                  ),
                ),
              ),
              // Spacer
              Spacer(),
              // 一行 3 列
              Container(
                child: Row(
                  crossAxisAlignment: CrossAxisAlignment.center,
                  children: <Widget>[
                    // 分类
                    ConstrainedBox(
                      constraints: BoxConstraints(
                        maxWidth: duSetWidth(60),
                      ),
                      child: Text(
                        item.category,
                        style: TextStyle(
                          fontFamily: 'Avenir',
                          fontWeight: FontWeight.normal,
                          color: AppColors.secondaryElementText,
                          fontSize: duSetFontSize(14),
                          height: 1,
                        ),
                        overflow: TextOverflow.clip,
                        maxLines: 1,
                      ),
                    ),
                    // 添加时间
                    Container(
                      width: duSetWidth(15),
                    ),
                    ConstrainedBox(
                      constraints: BoxConstraints(
                        maxWidth: duSetWidth(100),
                      ),
                      child: Text(
                        '• ${duTimeLineFormat(item.addtime)}',
                        style: TextStyle(
                          fontFamily: 'Avenir',
                          fontWeight: FontWeight.normal,
                          color: AppColors.thirdElementText,
                          fontSize: duSetFontSize(14),
                          height: 1,
                        ),
                        overflow: TextOverflow.clip,
                        maxLines: 1,
                      ),
                    ),
                    // 更多
                    Spacer(),
                    InkWell(
                      child: Icon(
                        Icons.more_horiz,
                        color: AppColors.primaryText,
                        size: 24,
                      ),
                      onTap: () {},
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ],
    ),
  );
}

```

## 资源

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### 参考

- https://pub.flutter-io.cn/packages/graphql
- https://strapi.io/documentation/v3.x/getting-started/quick-start.html

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
