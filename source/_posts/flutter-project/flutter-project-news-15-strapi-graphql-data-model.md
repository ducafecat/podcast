---
title: Flutter 实战从零开始 新闻客户端 - 15 strapi 数据建模 graphql 条件查询排序
date: 2020-07-09 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](2020-07-09-21-27-16.png)

# 本节目标

- portainer 容器管理工具
- 数据库设计过程
- 数据库设计目标、规范、习惯
- graphql 条件查询、排序
- flutter 代码实现

## 视频

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.15

## strapi 运行环境网盘下载

- 网盘

链接:https://pan.baidu.com/s/13Ujy2hzXp8tSqxCx_4IhVQ
密码:yu82

- 文件

| 名称                         | 说明                          |
| ---------------------------- | ----------------------------- |
| strapi-docker-compose-00.zip | 干净环境，已安装 graphql 插件 |
| strapi-docker-compose-15.zip | 15 课内容                     |

- 运行

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

### 安装 portainer

```sh
docker run -d -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --name portainer-local \
    portainer/portainer
```

### 设计数据模型

- 标准数据库设计

ER 图 -> 设计范式 -> 数据库物理表

- ER

![](2020-07-09-17-59-37.png)

- 范式

https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A7%84%E8%8C%83%E5%8C%96

- 设计规范 表前缀

sys* 系统、用户、权限
dict* 字典表
bus\_ 业务

- 设计数据 对象、属性、关系

![](2020-07-09-17-56-48.png)

### 创建 strapi 数据类型

- 创建外键表 新闻分类

![](2020-07-09-18-02-49.png)

- 创建外键表 新闻频道

![](2020-07-09-18-03-04.png)

- 创建业务表 新闻内容

![](2020-07-09-18-03-20.png)

- 创建链接外键 新闻内容、分类、频道

![](2020-07-09-18-03-37.png)

### 编写 graphql 查询

- 新闻

```graphql
query News($category_code: String) {
  busNews(where: { dict_categories: { code: $category_code } }) {
    title
    dict_channel {
      code
      title
      icon {
        url
      }
    }
    dict_categories {
      code
      title
    }
    author
    url
    addtime
    thumbnail {
      url
    }
  }
}
```

- 首页

```graphql
query pageIndex {
  # 分类
  dictCategories(sort: "sortNum:desc") {
    code
    title
  }
  # 频道
  dictChannels(sort: "sortNum:desc") {
    code
    title
    icon {
      url
    }
  }
  # 热点
  busNews(where: { dict_categories: { code: "news_hot" } }) {
    title
    dict_channel {
      code
      title
      icon {
        url
      }
    }
    dict_categories {
      code
      title
    }
    author
    url
    addtime
    thumbnail {
      url
    }
  }
}
```

### 编写 flutter 代码

- 实例 entity

lib/common/entitys/gql_news.dart

```dart
// 首页
class GqlIndexResponseEntity {
  GqlIndexResponseEntity({
    this.dictCategories,
    this.dictChannels,
    this.busNews,
  });

  List<DictCategoryEntity> dictCategories;
  List<DictChannelEntity> dictChannels;
  List<GqlNewsResponseEntity> busNews;

  factory GqlIndexResponseEntity.fromJson(Map<String, dynamic> json) =>
      GqlIndexResponseEntity(
        dictCategories: List<DictCategoryEntity>.from(
            json["dictCategories"].map((x) => DictCategoryEntity.fromJson(x))),
        dictChannels: List<DictChannelEntity>.from(
            json["dictChannels"].map((x) => DictChannelEntity.fromJson(x))),
        busNews: List<GqlNewsResponseEntity>.from(
            json["busNews"].map((x) => GqlNewsResponseEntity.fromJson(x))),
      );

  Map<String, dynamic> toJson() => {
        "dictCategories":
            List<dynamic>.from(dictCategories.map((x) => x.toJson())),
        "dictChannels": List<dynamic>.from(dictChannels.map((x) => x.toJson())),
        "busNews": List<dynamic>.from(busNews.map((x) => x.toJson())),
      };
}

// 新闻
class GqlNewsResponseEntity {
  GqlNewsResponseEntity({
    this.title,
    this.dictChannel,
    this.dictCategories,
    this.author,
    this.url,
    this.addtime,
    this.thumbnail,
  });

  String title;
  DictChannelEntity dictChannel;
  List<DictCategoryEntity> dictCategories;
  String author;
  String url;
  DateTime addtime;
  ThumbnailEntity thumbnail;

  factory GqlNewsResponseEntity.fromJson(Map<String, dynamic> json) =>
      GqlNewsResponseEntity(
        title: json["title"],
        dictChannel: DictChannelEntity.fromJson(json["dict_channel"]),
        dictCategories: List<DictCategoryEntity>.from(
            json["dict_categories"].map((x) => DictCategoryEntity.fromJson(x))),
        author: json["author"],
        url: json["url"],
        addtime: DateTime.parse(json["addtime"]),
        thumbnail: ThumbnailEntity.fromJson(json["thumbnail"]),
      );

  Map<String, dynamic> toJson() => {
        "title": title,
        "dict_channel": dictChannel.toJson(),
        "dict_categories":
            List<dynamic>.from(dictCategories.map((x) => x.toJson())),
        "author": author,
        "url": url,
        "addtime":
            "${addtime.year.toString().padLeft(4, '0')}-${addtime.month.toString().padLeft(2, '0')}-${addtime.day.toString().padLeft(2, '0')}",
        "ThumbnailEntity": thumbnail.toJson(),
      };
}

// 分类
class DictCategoryEntity {
  DictCategoryEntity({
    this.code,
    this.title,
  });

  String code;
  String title;

  factory DictCategoryEntity.fromJson(Map<String, dynamic> json) =>
      DictCategoryEntity(
        code: json["code"],
        title: json["title"],
      );

  Map<String, dynamic> toJson() => {
        "code": code,
        "title": title,
      };
}

// 频道
class DictChannelEntity {
  DictChannelEntity({
    this.code,
    this.title,
    this.icon,
  });

  String code;
  String title;
  ThumbnailEntity icon;

  factory DictChannelEntity.fromJson(Map<String, dynamic> json) =>
      DictChannelEntity(
        code: json["code"],
        title: json["title"],
        icon: ThumbnailEntity.fromJson(json["icon"]),
      );

  Map<String, dynamic> toJson() => {
        "code": code,
        "title": title,
        "icon": icon.toJson(),
      };
}

// 图
class ThumbnailEntity {
  ThumbnailEntity({
    this.url,
  });

  String url;

  factory ThumbnailEntity.fromJson(Map<String, dynamic> json) =>
      ThumbnailEntity(
        url: json["url"],
      );

  Map<String, dynamic> toJson() => {
        "url": url,
      };
}

```

- api

lib/common/apis/gql_news.dart

```dart
/// 新闻
class GqlNewsAPI {
  /// 首页
  static Future<GqlIndexResponseEntity> indexPageInfo({
    @required BuildContext context,
    Map<String, dynamic> params,
  }) async {
    QueryResult response =
        await GraphqlClientUtil.query(context: context, schema: GQL_INDEX_PAGE);

    return GqlIndexResponseEntity.fromJson(response.data);
  }

  /// 翻页
  static Future<List<GqlNewsResponseEntity>> newsPageList({
    @required BuildContext context,
    Map<String, dynamic> params,
  }) async {
    QueryResult response = await GraphqlClientUtil.query(
        context: context, schema: GQL_NEWS_LIST, variables: params);

    return response.data['busNews']
        .map<GqlNewsResponseEntity>(
            (item) => GqlNewsResponseEntity.fromJson(item))
        .toList();
  }
}
```

- 界面业务代码

lib/pages/main/main.dart

```dart
  GqlIndexResponseEntity _indexPageData; // 首页数据

  // 读取所有数据
  _loadAllData() async {
    _indexPageData = await GqlNewsAPI.indexPageInfo(context: context);
    ...

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
    _newsPageList = await GqlNewsAPI.newsPageList(
        context: context, params: {"category_code": categoryCode});

    if (mounted) {
      setState(() {});
    }
  }

  ...
```

> 详见 git

## 资源

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### 代码

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
