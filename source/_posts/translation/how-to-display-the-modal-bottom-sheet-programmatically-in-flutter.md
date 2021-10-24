---
title: Flutter 模式对话框
tags: flutter
categories: 译文
date: 2021-10-25 00:00:00
---

![](2021-10-24-22-22-54.png)

## 原文

> https://betterprogramming.pub/how-to-display-the-modal-bottom-sheet-programmatically-in-flutter-d1fad5fdd462

## 代码

https://github.com/macro6461/flutter_to_do

## 参考

- https://stackoverflow.com/questions/69471054/unable-to-reflect-updated-parent-state-in-showmodalbottomsheet
- https://stackoverflow.com/questions/69486752/trigger-floatingactionbutton-onpressed-without-pressing-the-button
- https://stackoverflow.com/users/1647098/eimmer

## 正文

如果你是从我以前的帖子来的，你看到我启动了我的待办事项应用程序，解决了我状态更新时不能用 `[showModalBottomSheet](https://api.flutter.dev/flutter/material/showModalBottomSheet.html)` 方法更新 `NewToDo` 的问题。

在 [Stack Overflow](https://stackoverflow.com/questions/69471054/unable-to-reflect-updated-parent-state-in-showmodalbottomsheet) 上问了一个问题之后，我终于可以把我的 `NewToDo` 小部件放到 `showModalBottomSheet` `中了。这样，NewToDo` 小部件只有在我单击 `FloatingActionButton` 时才可见，而不是必须一直可见。

然而，在实现这个目标后不久，我注意到我的应用程序中还有一个问题。

当我选择要编辑的 `ToDo` 项目时，我希望能够重用我的 `NewToDo`。我认为这是有意义的，因为它是同样的两个输入，可以用来改变相同的两个状态值，`title` 和 `content`。

问题是什么？

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855)

除了在我的 `FloatingActionButton` 小部件(绿色圆圈)的 `onPressed` 方法之外，我无法在任何地方执行 `showModalBottomSheet` (绿色箭头)。

我需要能够触发 `onPressed` 方法，每当我点击编辑标题 `ElevatedButton` 小部件(红色圆圈标记) ，每个 `ToDo` 项目。

我考虑了如何找到一种方法来模拟 `onPressed` 事件，以便执行 `showmodbottomsheet` 回调。

由于无法模拟 `onPressed` 事件(and frustrated) ，我带着 [Stack Overflow](https://stackoverflow.com/questions/69486752/trigger-floatingactionbutton-onpressed-without-pressing-the-button) 去看看是否有其他人对如何实现我所寻找的目标有任何想法。

过了一会儿，我得到了答案。我松了一口气... ... 也谦卑起来。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/74c7766323afc2ca5ab2947123fe7e1c9ba3ad8a606498184c9ed660b4557304.png)

如此简单... 如此纯洁

我采纳了 [eimmer’s](https://stackoverflow.com/users/1647098/eimmer) 的建议，没有把我的 `showModalBottomSheet` 放在 `onPressed` 方法中，而是把它分解成了它自己的函数 `renderShowModal`。参见下文:

```dart
_renderShowModal(){
  return showModalBottomSheet<void>(
    context: context,
    builder: (BuildContext context) {
      return ValueListenableBuilder(
        valueListenable: titleController,
        builder: (context, _content, child) {
          return NewToDo(titleController, contentController, _addTodo, _clear, _todo);
        });
    },
  );
}
```

这样做之后，我就可以重写我的 `onPressed` 方法了。

```dart
onPressed: _renderShowModal,
```

现在我们已经把我们的 `showModalBottomSheet` 放在单独的函数中，让我们看看它是否工作:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/fd0285b5c0e06946152c727f089961de42f6ff92f6e32c329df51809b3677d7f.gif)

太棒了！我们现在需要做的就是在 `editTodo` 函数的末尾调用相同的函数，当你点击编辑 `EelevatedButton` 时，这个函数就会被调用。

```dart
void _editTodo(ToDo todoitem){
  setState(() {
    _todo = todoitem;
    content = todoitem.content;
    title = todoitem.title;
  });
  contentController.text = todoitem.content;
  titleController.text = todoitem.title;
  _renderShowModal();
}
```

现在让我们看看当我们单击 `ToDo` 项目旁边的 `edit` 时它是否工作。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5be62e8344de3b6c18a1956234169f5fb24cd7cb7c3944bdcf82e1842336a367.gif)

Viola! 维奥拉

我们现在可以使用 `NewToDo` 来创建新的 `ToDo` 项目和编辑现有的 `ToDo` 项目。

通过将 `showModalBottomSheet` 包装在一个单独的函数中，我们可以通过调用 `renderShowModal`，让我们的应用程序在任何时候都可以呈现 `NewToDo` invoking `_renderShowModal`。

这个应用的所有代码都在 [GitHub](https://github.com/macro6461/flutter_to_do) 上。

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
