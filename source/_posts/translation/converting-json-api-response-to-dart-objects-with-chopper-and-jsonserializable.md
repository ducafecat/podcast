---
title: 转换 JSON API 用 Chopper 和 JsonSerializable
tags: flutter
categories: 译文
date: 2021-07-08 00:00:00
---

![](2021-07-08-09-30-02.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/teamkraken/converting-json-api-response-to-dart-objects-with-chopper-and-jsonserializable-8ec98b762ac1

## 代码

https://github.com/ErkinKurt/chopper_json_serializable

## 参考

- https://hadrien-lejard.gitbook.io/chopper/
- https://github.com/infinum/Japx
- https://jsonapi.org/
- https://github.com/dart-lang/build

## 正文

不过，JSON 响应格式因服务而异; 有一些共享的约定，比如 JSON: API，HAL... 今天，我将尝试展示如何从头开始，将 JSON: API 响应转换为 Flutter 项目中的 Dart 对象。

如果你想跟随源代码，这里是: Chopper _ json _ serializable

https://github.com/ErkinKurt/chopper_json_serializable

### 1. 创建项目并添加依赖项

将要使用的软件包:

- Chopper:
  使用 source gen 和 Retrofit 的 Dart 和 Flutter 的 Http 客户端生成器

- Json Serializable:
  通过使用它，将自动生成实体的 JSON 转换

- Japx:
  解析器，它将复杂的[ JSON: API ][1]结构变为简单的 JSON，反之亦然

非常感谢这些令人敬畏的包裹！

由于 JsonSerializable 和 Chopper 使用 meta 编程，他们需要 BuildRunner 来处理代码生成。下面是要使用的 pubspec.yaml 文件:

![](2021-07-08-06-16-57.png)

### 2. 实现 Mock Http Client 以获得 JSON: API 格式的响应

响应模型在 assets 文件夹中提供，因为我们希望使用 mock http 客户机，并且不依赖于任何远程源。响应模型取自 JAPX 资产。

如果你想使用 chopper 与常规的 http 客户端只是忽略这一部分，并沿着以下部分..。

![](2021-07-08-06-17-53.png)

### 3. 创建模型并使用 JsonSerializable

尽管为了简单起见，我们有两个实体 Article 和 People，但是它们将从 Entity 类继承，以演示如何将 JsonSerializable 与继承一起使用。因为每个实体在 JSON: API 中都有一个 type 和 id 值，所以我们创建了一个具有 type 和 id 的基类。

```dart
abstract class Entity {
  final String type;
  final String id;

  Entity(this.type, this.id);
}
```

让我们用 JsonSerializable 实现两个模型。我们需要编写 part’< model-name > 。在一天结束的时候，json 和 FromJson 构造函数将由这个目录中构建的 runner 生成。

我们正在将 fromJsonFactory 定义为在 JsonTypeConvertor 中使用，稍后我们将看到它的使用。

![](2021-07-08-06-18-57.png)

在完成我们的实现之后，我们需要在项目的根目录中运行 flutter pub run build runner build，这样 build runner 就可以执行它的魔术了。在成功的执行之后，我们看到。G 文件生成，没有错误。

### 4. 创建 Chopper 服务

让我们创建网络层与 Chopper 服务。Chopper Services 使用自动生成来创建底层的 http 客户端实现，因此我们不需要担心 http 客户端请求。

我们提供了一个静态创建方法，将服务注入到 ChopperClients 中。除此之外，我们还有通过 http 客户端获取单个实体的 getArticle 和 getPerson 方法。

![](2021-07-08-06-19-34.png)

正如你所看到的，我们有一些红灯亮着。这是因为 chopper 在 build _ runner 的帮助下使用代码生成。(build _ runner the underlying hero).

让我们像之前一样运行 flutter pub run build \_ runner build 命令，如果执行成功，我们不会看到任何错误。请看一下生成的代码，看看 chopper 是如何处理基础 http 客户端请求的。

### 5. 实现 Chopper Client

现在我们需要实现 Chopper 客户端使用我们创建的 Chopper 服务。Chopper Client 提供服务属性，设置多种服务。对于客户端使用，有两个选项，要么创建客户端为单例，并传递所有服务，要么在任何需要的时候创建客户端。

对于这个示例，让我们创建一个 chopper 客户机实例，并将其传递给 widget 树。然而，我们可以使用 getIt 或者 provider 作为依赖注入，为了简单起见，我们将其作为支柱。

下面是一个 ChopperClientBuilder，它为我们的 ChopperClientBuilder 客户端提供了构建方法。

![](2021-07-08-06-20-20.png)

![](2021-07-08-06-20-29.png)

### 6. 使用用户界面中的 JAPX 解码器使用 Chopper 服务

我们有一个带 chopper client 作为 prop 的有状态小部件，我们将实现两个方法 getArticle 和 getPeople。在这一部分中，JAPX 扮演了重要的角色。因为我们的 JSON 响应具有 JSON: API 响应格式，所以我们需要平滑复杂的响应。

![](2021-07-08-06-20-57.png)

然而，上面的例子可以很好地工作; 我们需要进行显式转换:

- `json.decode` to get mapped json object
- `Japx.decode` to flatten the map.
- Instantiate dart object with our model’s `fromJsonFactory` methods.

这种方法有两个问题:

- 显式转换将导致代码重复
- 服务方法 Response<dynamic> return type.

![](2021-07-08-06-23-42.png)

让我们看看如何处理这些问题..。

### 7. 为响应类型规范实现 JsonConvertor

以某种方式，我们需要操作 Chopper Responses 来返回我们之前定义的模型类型。由于称为转换器的特殊拦截器，我们可以处理响应、请求和错误的转换。所有我们需要做的是创建转换器，并注入到 Chopper 客户端，我们使用。

让我们为 Chopper Client 创建 jsonserializableeconverter 类。为了做到这一点，我们需要从 Chopper 的 JsonConverter 类扩展我们的类，并重写 convertResponse、 convertRequest 和 convertError 方法。

![](2021-07-08-06-24-23.png)

现在我们在 convertResponse 方法中进行转换，这也解决了代码重复问题，因为每次成功响应都会调用这个方法。但是，仍然有一个类型转换问题需要解决。

```dart
jsonRes.copyWith<ResultType>(body: JsonTypeParser.decode<Item>(flatJson["data"]))
```

这是解析 Dart 模型响应的关键语句:

- 实际上是我们希望服务方法使用的返回类型 ResultType 是 List or 或 BuiltList 会是我们的回归类型。为了在服务方法中定义返回类型，我们需要添加我们期望的模型类型

![](2021-07-08-06-26-24.png)

在此更改之后，无论何时触发 convertResponse，ResultType 都将是 People 或 Article。

- 然而，我们仍然需要调用我们的模型的 fromJsonFactory 方法将响应对象转换为 Dart 对象。为了做到这一点，我们创建 JsonTypeParser 类，它将保存 Dart 对象的转换逻辑

![](2021-07-08-06-27-15.png)

JsonTypeParser 有 Map < Type，JsonFactory > 类型的 factories 属性，其中包含来自 JsonFactory 方法的模型和模型。每当我们有一个新的模型，我们需要添加它的类型和工厂到工厂，以便它可以通过 \_ decodeMap 方法解析。JsonSerializableConverter 执行 decode < t > 方法，t 为 ResultType 或 Item，并调用匹配类型的 factory get 来实例化我们的模型。

让我们跟随这个变化和修改用户界面部分，我们消费斩波服务..。

让我们把 jsonserializableeconverter 插入到我们的 chopper 客户端:

![](2021-07-08-06-27-46.png)

根据这些变化，现在我们可以直接使用响应体，具有类型安全性。

![](2021-07-08-06-28-02.png)

### 总结

我知道这有很多东西需要消化，但是，还有很多东西需要讨论，比如 JsonSerializable 和 Chopper 的响应错误的自定义类型转换器。如果我写一篇中等程度的文章，我会把这个话题连接起来。

总而言之，我想要展示的是各种响应模型可以通过斩波和 json 序列化解决，而不会失去类型安全的清洁方式。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
