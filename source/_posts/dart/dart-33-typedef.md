---
title: Dart语言学习 - 33 类型信息 typedef
date: 2019-01-21 00:21:06
tags: dart
categories: Dart语言学习
---

# 本节目标

- typedef 使用

# 环境

- Dart 2.1.0

# 作用

typedef 用来保存函数的信息，未来可能会保存类信息。

# 示例代码

- 采用 `typedef`

```dart
// 定义函数类型
typedef int Compare(Object a, Object b);

// 定义排序类
class SortedCollection {
  Compare compare;
  // 构造时传入函数
  SortedCollection(this.compare);
}

// 定义排序函数
int sort(Object a, Object b) => 0;

// 程序入口
main() {
  // 实例化传入
  SortedCollection coll = new SortedCollection(sort);
  // 类型检查
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

- 未采用 `typedef`

```dart
class SortedCollection {
  // 函数对象
  Function compare;
  
  // 定义函数
  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// 生命函数
int sort(Object a, Object b) => 0;

main() {
  // 实例化
  SortedCollection coll = new SortedCollection(sort);

  // 我们只知道 compare 是一个 Function 类型，
  // 但是不知道具体是何种 Function 类型？
  assert(coll.compare is Function);
}
```

区别就是 `typedef` 编辑器会提示函数信息

# 代码

- [33-类型信息](https://github.com/ducafecat/dart-learn/tree/master/33-%E7%B1%BB%E5%9E%8B%E4%BF%A1%E6%81%AF)

# 参考

- [typedefs](https://www.dartlang.org/guides/language/language-tour#typedefs)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
