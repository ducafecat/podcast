---
title: Dart语言学习 - 33 类型信息 typedef
date: 2019-01-21 17:21:06
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
typedef int Compare(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

- 未采用

```dart
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

main() {
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
