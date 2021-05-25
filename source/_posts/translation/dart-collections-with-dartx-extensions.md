---
title: Dart 集合操作插件 DartX
tags: flutter
categories: 译文
date: 2021-05-25 06:13:25
---

![](2021-05-25-08-07-06.png)

> 本次图片是向 kallehallden 致敬，热爱编程，保持一颗好奇的心

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 猫哥说

最近没时间录视频，一直在做项目和技术研究，就翻译和写写文章和大家分享。

关于这篇文章，我只想说一切让我们少写代码，让代码简洁的方式都是好东西！

也许这个组件 dartx 在某些人眼里不够成熟，但是这代表了一种思路，你应该去借鉴。

## 原文

> https://medium.com/flutter-community/dart-collections-with-dartx-extensions-959a0b42849e

## 参考

- https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/linq-to-objects
- https://pub.dev/packages/dartx

## 正文

Darts Iterable 和 List 提供了一组基本的方法来修改和查询集合。然而，来自 c # 背景，它有一个非凡的 LINQ-to-Objects 库，我觉得我日常使用的一部分功能缺失了。当然，任何任务都可以只使用 dart.core 方法来解决，但有时需要多行解决方案，而且代码不是那么明显，需要花费精力去理解。我们都知道，开发人员花费大量时间阅读代码，使代码简洁明了是至关重要的。

![](2021-05-25-06-15-25.png)

Dartx 包允许开发人员在集合(和其他 Dart 类型)上使用优雅且可读的单行操作。

https://pub.dev/packages/dartx

下面我们将比较这些任务是如何解决的:

- first / last collection item or null / default value / by condition;
- map / filter / iterate collection items depending on their index;
- converting a collection to a map;
- sorting collections;
- collecting unique items;
- min / max / average by items property;
- filtering out null items;

### 安装 dartx `pubspec.yaml`

```yaml
dependencies:
  dartx: ^0.7.1
```

```dart
import 'package:dartx/dartx.dart';
```

### First / last collection item…

#### … or null

为了得到第一个和最后一个收集项目的简单省道，你可以这样写:

```dart
final first = list.first;
final last = list.last;
```

如果 list 为空，则抛出 statereerror，或者显式返回 null:

```dart
final firstOrNull = list.isNotEmpty ? list.first : null;
final lastOrNull = list.isNotEmpty ? list.last : null;
```

使用 dartx 可以:

```dart
final firstOrNull = list.firstOrNull;
final lastOrNull = list.lastOrNull;
```

类似地:

```dart
final elementAtOrNull = list.elementAtOrNull(index);
```

> 如果索引超出列表的界限，则返回 null。

#### … or default value

鉴于你现在记住这一点。第一和。当 list 为空时，last getter 会抛出错误，以获取第一个和最后一个集合项或默认值，在简单的 Dart 中，你会写:

```dart
final firstOrDefault = (list.isNotEmpty ? list.first : null) ?? defaultValue;

final lastOrDefault = (list.isNotEmpty ? list.last : null) ?? defaultValue;
```

使用 dartx 可以:

```dart
final firstOrDefault = list.firstOrDefault(defaultValue);

final lastOrDefault = list.lastOrElse(defaultValue);
```

类似于 elementAtOrNull:

```dart
final elementAtOrDefault = list.elementAtOrDefault(index, defaultValue);
```

> 如果索引超出列表的边界，则返回 defaultValue。

#### … by condition

要获取第一个和最后一个符合某些条件或 null 的集合项，一个普通的 Dart 实现应该是:

```dart
final firstWhere = list.firstWhere((x) => x.matches(condition));

final lastWhere = list.lastWhere((x) => x.matches(condition));
```

除非提供 orElse，否则它将为空列表抛出 StateError:

```dart
final firstWhereOrNull = list.firstWhere((x) => x.matches(condition), orElse: () => null);

final lastWhereOrNull = list.lastWhere((x) => x.matches(condition), orElse: () => null);
```

使用 dartx 可以:

```dart
final firstWhereOrNull = list.firstOrNullWhere((x) => x.matches(condition));

final lastWhereOrNull = list.lastOrNullWhere((x) => x.matches(condition));
```

### … collection items depending on their index

#### Map…

当您需要获得一个新的集合，其中每个项目以某种方式依赖于其索引时，这种情况并不罕见。例如，每个新项都是来自原始集合的项及其索引的字符串表示形式。

如果你喜欢我的一句俏皮话，简单地说就是:

```dart
final newList = list.asMap()
  .map((index, x) => MapEntry(index, '$index $x'))
  .values
  .toList();
```

使用 dartx 可以:

```dart
final newList = list.mapIndexed((index, x) => '$index $x').toList();
```

> 我应用.toList ()是因为这个和大多数其他扩展方法返回 lazy Iterable。

#### Filter...

对于另一个示例，假设只需要收集奇数索引项。使用简单省道，可以这样实现:

```dart
final oddItems = [];
for (var i = 0; i < list.length; i++) {
  if (i.isOdd) {
    oddItems.add(list[i]);
  }
}
```

或者用一行代码:

```dart
final oddItems = list.asMap()
  .entries
  .where((entry) => entry.key.isOdd)
  .map((entry) => entry.value)
  .toList();
```

使用 dartx 可以:

```dart
final oddItems = list.whereIndexed((x, index) => index.isOdd).toList();

// or

final oddItems = list.whereNotIndexed((x, index) => index.isEven).toList();
```

### Iterate…

如何记录集合内容并指定项目索引？

In plain Dart:

```dart
for (var i = 0; i < list.length; i++) {
  print('$i: ${list[i]}');
}
```

使用 dartx 可以:

```dart
list.forEachIndexed((element, index) => print('$index: $element'));
```

### Converting a collection to a map

例如，需要将不同 Person 对象的列表转换为 Map < String，Person > ，其中键是 Person.id，值是完整 Person 实例。

```dart
final peopleMap = people.asMap().map((index, person) => MapEntry(person.id, person));
```

使用 dartx 可以:

```dart
final peopleMap = people.associate((person) => MapEntry(person.id, person));

// or

final peopleMap = people.associateBy((person) => person.id);
```

要得到一个 Map，其中键是 DateTime，值是列出那天出生的人的名单 < person > ，在简单的 Dart 中，你可以写:

```dart
final peopleMapByBirthDate = people.fold<Map<DateTime, List<Person>>>(
  <DateTime, List<Person>>{},
  (map, person) {
    if (!map.containsKey(person.birthDate)) {
      map[person.birthDate] = <Person>[];
    }
    map[person.birthDate].add(person);
    return map;
  },
);
```

使用 dartx 可以:

```dart
final peopleMapByBirthDate = people.groupBy((person) => person.birthDate);
```

### Sorting collections

你会怎样用普通的 dart 来分类一个收集? 你必须记住这一点

```dart
list.sort();
```

修改源集合，要得到一个新实例，你必须写:

```dart
final orderedList = [...list].sort();
```

Dartx 提供了一个扩展来获得一个新的 List 实例:

```dart
final orderedList = list.sorted();

// and

final orderedDescendingList = list.sortedDescending();
```

如何基于某些属性对收集项进行排序？

Plain Dart:

```dart
final orderedPeople = [...people]
  ..sort((person1, person2) => person1.birthDate.compareTo(person2.birthDate));
```

使用 dartx 可以:

```dart
final orderedPeople = people.sortedBy((person) => person.birthDate);

// and

final orderedDescendingPeople = people.sortedByDescending((person) => person.birthDate);
```

更进一步，你可以通过多个属性对集合项进行排序:

```dart
final orderedPeopleByAgeAndName = people
  .sortedBy((person) => person.birthDate)
  .thenBy((person) => person.name);

// and

final orderedDescendingPeopleByAgeAndName = people
  .sortedByDescending((person) => person.birthDate)
  .thenByDescending((person) => person.name);
```

### Collecting unique items

要获得不同的集合项，可以使用以下简单的 Dart 实现:

```dart
final unique = list.toSet().toList();
```

这并不保证保持项目顺序或提出一个多行的解决方案

使用 dartx 可以:

```dart
final unique = list.distinct().toList();

// and

final uniqueFirstNames = people.distinctBy((person) => person.firstName).toList();
```

### Min / max / average by item property

例如，要查找一个 min/max 集合项，我们可以对其进行排序，并获取 first/last 项:

```dart
final min = ([...list]..sort()).first;

final max = ([...list]..sort()).last;
```

同样的方法也适用于按项属性进行排序:

```dart
final minAge = (people.map((person) => person.age).toList()..sort()).first;

final maxAge = (people.map((person) => person.age).toList()..sort()).last;
```

或:

```dart
final youngestPerson =
  ([...people]..sort((person1, person2) => person1.age.compareTo(person2.age))).first;

final oldestPerson =
  ([...people]..sort((person1, person2) => person1.age.compareTo(person2.age))).last;
```

使用 dartx 可以:

```dart
final youngestPerson = people.minBy((person) => person.age);

final oldestPerson = people.maxBy((person) => person.age);
```

对于空集合，它将返回 null。

如果收集项目实现了 Comparable，则可以应用不带选择器的方法:

```dart
final min = list.min();

final max = list.max();
```

你也可以很容易地得到平均值:

```dart
final average = list.average();

// and

final averageAge = people.averageBy((person) => person.age);
```

以及 num 集合或 num 项属性的总和:

```dart
final sum = list.sum();

// and

final totalChildrenCount = people.sumBy((person) => person.childrenCount);
```

### Filtering out null items

With plain Dart:

```dart
final nonNullItems = list.where((x) => x != null).toList();
```

使用 dartx 可以:

```dart
final nonNullItems = list.whereNotNull().toList();
```

### More useful extensions

在 dartx 中还有其他有用的扩展。这里我们不会深入讨论更多细节，但是我希望命名和代码是不言自明的。

#### joinToString

```dart
final report = people.joinToString(
  separator: '\n',
  transform: (person) => '${person.firstName}_${person.lastName}',
  prefix: '<<️',
  postfix: '>>',
);
```

#### all (every) / none

```dart
final allAreAdults = people.all((person) => person.age >= 18);

final allAreAdults = people.none((person) => person.age < 18);
```

#### first / second / third / fourth collection items

```dart
final first = list.first;
final second = list.second;
final third = list.third;
final fourth = list.fourth;
```

#### takeFirst / takeLast

```dart
final youngestPeople = people.sortedBy((person) => person.age).takeFirst(5);

final oldestPeople = people.sortedBy((person) => person.age).takeLast(5);
```

#### firstWhile / lastWhile

```dart
final orderedPeopleUnder50 = people
  .sortedBy((person) => person.age)
  .firstWhile((person) => person.age < 50)
  .toList();

final orderedPeopleOver50 = people
  .sortedBy((person) => person.age)
  .lastWhile((person) => person.age >= 50)
  .toList();
```

### 总结

Dartx 包包含了许多针对 Iterable、 List 和其他 Dart 类型的扩展。探索其功能的最佳方式是浏览源代码。

https://github.com/leisim/dartx

感谢软件包作者 Simon Leier 和 Pascal Welsch。

https://www.linkedin.com/in/simon-leier/

https://twitter.com/passsy

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
