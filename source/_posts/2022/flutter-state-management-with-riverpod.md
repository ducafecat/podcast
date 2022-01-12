---
title: 基于 Riverpod 的 Flutter 状态管理
tags: flutter
categories: 译文
date: 2022-01-12 00:00:00
---

![](2022-01-12-22-11-50.png)

## 原文

> https://itnext.io/flutter-state-management-with-riverpod-ef8d4ef77392

## 代码

https://github.com/iisprey/riverpod_example

## 参考

- https://itnext.io/a-minimalist-guide-to-riverpod-4eb24b3386a1
- https://pub.dev/packages/state_notifier
- https://iisprey.medium.com/get-rid-of-all-kind-of-boilerplate-code-with-flutter-hooks-2e17eea06ca0
- https://iisprey.medium.com/how-to-handle-complex-json-in-flutter-4982015b4fdf

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/17628a0cd1c5ce558fb867f7ec7100c2dc740d1eb3b6a4effecbd20aeabf3379.jpeg)

正如我上周所承诺的，我将向您展示我自己的最终国家管理解决方案路径

### [Riverpod](/a-minimalist-guide-to-riverpod-4eb24b3386a1) + [StateNotifier](https://pub.dev/packages/state_notifier) + [Hooks](https://iisprey.medium.com/get-rid-of-all-kind-of-boilerplate-code-with-flutter-hooks-2e17eea06ca0) + [Freezed](https://iisprey.medium.com/how-to-handle-complex-json-in-flutter-4982015b4fdf)

Riverpod 太棒了！但是好的例子并不多。只有最基本的，就这样。这一次，我试图使一个例子既可理解又复杂。我的目的是通过这个例子教你什么时候使用 Riverpod，以及如何使用它。尽管我简化了过程。希望你喜欢！

### 动机

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/14e64ed984810ff1e2f2ea2a5e9f6a51059e31730a058c6226cb678d614a7dd8.gif)

### 在这个例子中我们要做什么？

我们只需要从 API 中获取一些数据，然后在 UI 中对它们进行排序和过滤

### 基本上，我们会;

1.  Create simple and complex providers and combine them
2.  创建简单和复杂的提供程序并将它们组合起来
3.  Use AsyncValue object and show async value in the UI using `when` method
4.  使用 AsyncValue 对象并在 UI 中使用 when 方法显示 async 值
5.  Also, create `freezed` objects for immutable object solution
6.  同时，为不可变物件/解决方案创建冻结对象

我们开始吧！

### 创建 API 服务

> 注意: 我没有找到一个好的 API 模型来使用过滤功能，因为我自己添加了这些角色。原谅我这么说

```dart
final userService = Provider((ref) => UserService());

class UserService {
  final _dio = Dio(BaseOptions(baseUrl: 'https://reqres.in/api/'));

  Future<List<User>> getUsers() async {
    final res = await _dio.get('users');
    final List list = res.data['data'];
    // API didn't have user roles I just added by hand (it looks ugly but never mind)
    list[0]['role'] = 'normal';
    list[1]['role'] = 'normal';
    list[2]['role'] = 'normal';
    list[3]['role'] = 'admin';
    list[4]['role'] = 'admin';
    list[5]['role'] = 'normal';
    return list.map((e) => User.fromJson(e)).toList();
  }
}
```

### 使用 `freezed` 和 `json_serializable` 创建不可变模型

我们只需要创建一个

```dart
part 'user.freezed.dart';
part 'user.g.dart';

@freezed
class User with _$User {
  @JsonSerializable(fieldRename: FieldRename.snake)
  const factory User({
    required int id,
    required String email,
    required String firstName,
    required String lastName,
    required String avatar,
    @JsonKey(unknownEnumValue: Role.normal) required Role role,
  }) = _User;

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
}
```

如果你想了解更多关于 freezed 的信息，请查看这篇文章。

https://iisprey.medium.com/how-to-handle-complex-json-in-flutter-4982015b4fdf

### 从服务中获得 `value`

你可以想想，`AsyncValue` 是什么? 它只是一个联合类，帮助我们处理我们的值的状态。它提供了一个现成的提供者包。

我将在接下来的文章中详细解释，但就目前而言，仅此而已。

```dart
final usersProvider = StateNotifierProvider.autoDispose<UserNotifier, AsyncValue<List<User>>>((ref) {
  return UserNotifier(ref);
});

class UserNotifier extends StateNotifier<AsyncValue<List<User>>> {
  final AutoDisposeStateNotifierProviderRef _ref;

  late final UserService _service;

  UserNotifier(this._ref) : super(const AsyncValue.data(<User>[])) {
    _service = _ref.watch(userService);
    getUsers();
  }

  Future<void> getUsers() async {
    state = const AsyncValue.loading();
    final res = await AsyncValue.guard(() async => await _service.getUsers());
    state = AsyncValue.data(res.asData!.value);
  }
}
```

### 创建排序和过滤器提供程序

```dart
enum Role { none, normal, admin }
enum Sort { normal, reversed }

final filterProvider = StateProvider.autoDispose<Role>((_) => Role.none);
final sortProvider = StateProvider.autoDispose<Sort>((_) => Sort.normal);
```

### 从提供程序获取获取的列表并进行筛选，然后使用其他提供程序对它们进行排序

```dart
final filteredAndSortedUsersProvider = Provider.autoDispose.family<List<User>, List<User>>((ref, users) {
  final filter = ref.watch(filterProvider);
  final sort = ref.watch(sortProvider);

  late final List<User> filteredList;

  switch (filter) {
    case Role.admin:
      filteredList = users.where((e) => e.role == Role.admin).toList();
      break;
    case Role.normal:
      filteredList = users.where((e) => e.role == Role.normal).toList();
      break;
    default:
      filteredList = users;
  }

  switch (sort) {
    case Sort.normal:
      return filteredList;
    case Sort.reversed:
      return filteredList.reversed.toList();
    default:
      return filteredList;
  }
});
```

### 在用户界面中显示所有内容

```dart
class HomePage extends ConsumerWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final users = ref.watch(usersProvider);
    return Scaffold(
      appBar: AppBar(
        centerTitle: true,
        title: const Text('Users'),
      ),
      body: RefreshIndicator(
        onRefresh: () async => await ref.refresh(usersProvider),
        child: users.when(
          data: (list) {
            final newList = ref.watch(filteredAndSortedUsersProvider(list));
            if (newList.isEmpty) {
              return const Center(child: Text('There is no user'));
            }
            return ListView.builder(
              itemCount: newList.length,
              itemBuilder: (_, i) {
                final user = newList[i];
                return ListTile(
                  minVerticalPadding: 25,
                  leading: Image.network(user.avatar),
                  title: Text('${user.firstName} ${user.lastName}'),
                  trailing: Text(user.role.name),
                );
              },
            );
          },
          error: (_, __) => const Center(child: Text('err')),
          loading: () => const Center(child: CircularProgressIndicator()),
        ),
      ),
      floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,
      floatingActionButton: Row(
        mainAxisSize: MainAxisSize.min,
        children: [
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Consumer(
                builder: (_, ref, __) {
                  final sort = ref.watch(sortProvider.state);
                  return ElevatedButton(
                    onPressed: () {
                      if (sort.state == Sort.reversed) {
                        sort.state = Sort.normal;
                      } else {
                        sort.state = Sort.reversed;
                      }
                    },
                    child: Text(
                      sort.state == Sort.normal
                          ? 'sort reversed'
                          : 'sort normal',
                    ),
                  );
                },
              ),
            ),
          ),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Consumer(
                builder: (_, ref, __) {
                  final filter = ref.watch(filterProvider.state);
                  return ElevatedButton(
                    onPressed: () {
                      if (filter.state == Role.admin) {
                        filter.state = Role.none;
                      } else {
                        filter.state = Role.admin;
                      }
                    },
                    child: Text(
                      filter.state == Role.admin
                          ? 'remove filter'
                          : 'filter admins',
                    ),
                  );
                },
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

## 结束

如果你把这个例子看作一个电子商务应用程序，那么这个例子就更有意义了

我不是 `riverpod` 的宗师。只是学习和分享我的经验，所以请如果你知道一个更好的方式使用 `riverpod` 请让我们知道！

## 示例 Github 项目

这是源代码。

https://github.com/iisprey/riverpod_example

谢谢你的阅读

---

© 猫哥

- [ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
