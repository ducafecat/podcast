---
title: Flutter ModalBottomSheet 指导
tags: flutter
categories: 译文
date: 2021-05-18 00:00:00
---

![](2021-05-18-09-22-09.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://evandrmb.medium.com/flutter-modalbottomsheet-for-beginners-e5f3af271076

## 代码

https://github.com/evandrmb/bottom_sheet

## 参考

- https://material.io/components/sheets-bottom

## 正文

根据材质设计指南，底部表是一个小工具，用于显示锚定在屏幕底部的附加内容。虽然了解使用这个的设计规则很好，但这不是本文的目标。要了解更多关于底板设计原则的详细信息，请参阅“[Sheets: bottom — Material Design](https://material.io/components/sheets-bottom)”。

现在你知道了 BottomSheet，你可能会问自己: 什么是 ModalBottomSheet？我们如何使用他们在 Flutter？

好的，第一个问题，有两种底层表: 模态的和持久的。当用户与屏幕交互时，持久化保持可见。谷歌地图应用就是一个例子。

另一方面，模式化的操作会阻止用户在应用程序中做其他动作。您可以使用它们来确认某些操作，或者请求额外的数据，比如询问用户在电子商务应用程序中订购时需要多少交换，等等。

在本文中，我们将通过创建一个简单的体重跟踪应用程序来展示如何使用它，在这个应用程序中我们可以提交我们的体重并查看我们之前的体重。我们不会输入应用程序的详细信息，而是直接进入 ModalBottomSheet 实现。

要显示它，您需要从具有 Scaffold 的上下文调用 showModalBottomSheet，否则，您将得到一个错误。也就是说，让我们开始构建我们的表格。

首先要知道的是 ModalBottomSheets 的高度默认为屏幕的一半，为了改变它，必须传递 true 给 isScrollControlled 参数，并返回一个与我们期望的大小相匹配的小部件，所以让我们这样做。

```dart
void addWeight() {
    showModalBottomSheet(
      isScrollControlled: true,
      context: context,
      builder: (context) {
        var date = DateTime.now();

		    return Container(
          height: 302,
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [

            ],
          ),
        );
      },
    );
  }
```

现在，我们需要添加一些东西，以便我们的用户可以输入他们的权重让我们添加一个 TextInput 并给它一个 TextEditingController (这种方式即使我们的工作表意外关闭时，用户再次打开它，它的值仍然存在)。

```dart
void addWeight() {
    showModalBottomSheet(
      isScrollControlled: true,
      context: context,
      builder: (context) {
        var date = DateTime.now();

		    return Container(
          height: 302,
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
				      padding: EdgeInsets.only(bottom: 24.0),
                child: Text(
                  'Register Weight',
                  style: Styles.titleStyle,
                ),
              ),
              TextField(
                controller: weightInputController,
                keyboardType: TextInputType.number,
                decoration: InputDecoration(
                  labelText: 'Weight (KG)',
                  border: OutlineInputBorder(
                    borderRadius: Styles.borderRadius,
                  ),
                ),
              ),
            ],
          ),
        );
      },
    );
  }
```

看起来不错，但现在我们有麻烦了。当用户点击我们的 TextField 键盘在它上面，为什么？当键盘打开时，我们的工作表不会调整位置，我们可以把工作表做得更大，但这不能解决我们的问题，因为我们仍然需要添加一个字段，用户可以在其中输入他们记录重量的日期。那么解决方案是什么呢？这很简单，如果打开键盘，我们让我们的工作表在它上面，我们可以实现这一点，给我们的容器一个边距的边缘。在 viewinset.bottom 中，我们将得到以下结果:

![](2021-05-18-08-58-03.png)

它开始看起来很漂亮，但是你不认为如果我们在纸上加一些半径会更平滑吗？让我们通过添加如下所示的 shapeproperty 来实现。

```dart
showModalBottomSheet(
      isScrollControlled: true,
      context: context,
      shape: const RoundedRectangleBorder(
          borderRadius: BorderRadius.only(
            topLeft: Radius.circular(8),
            topRight: Radius.circular(8),
          )),
```

酷，现在让我们做我们的小工具来选择一个日期。通常，您会创建一个小部件来处理这个逻辑，并使用 ValueChanged 函数公开选定的值，但是为了说明您将来可能面临的问题，让我们在工作表本身内部创建所有逻辑。

```dart
void addWeight() {
    showModalBottomSheet(
      isScrollControlled: true,
      context: context,
      shape: const RoundedRectangleBorder(
          borderRadius: BorderRadius.only(
        topLeft: Radius.circular(8),
        topRight: Radius.circular(8),
      )),
      builder: (context) {
        return Container(
          height: 360,
          width: MediaQuery.of(context).size.width,
          margin:
              EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom),
          padding: const EdgeInsets.all(16.0),
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(20),
          ),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              const Padding(
                padding: EdgeInsets.only(bottom: 24.0),
                child: Text(
                  'Register Weight',
                  style: Styles.titleStyle,
                ),
              ),
              TextField(
                controller: weightInputController,
                keyboardType: TextInputType.number,
                decoration: InputDecoration(
                  labelText: 'Weight (KG)',
                  border: OutlineInputBorder(
                    borderRadius: Styles.borderRadius,
                  ),
                ),
              ),
              Padding(
                padding: const EdgeInsets.symmetric(vertical: 8.0),
                child: Row(
                  children: [
                    Expanded(
                      child: Text(
                        'Select a date',
                        style: TextStyle(
                          fontSize: 14,
                          fontWeight: FontWeight.w500,
                        ),
                      ),
                    ),
                    Container(
                      padding: const EdgeInsets.symmetric(horizontal: 4),
                      margin: const EdgeInsets.symmetric(vertical: 8.0),
                      height: 36,
                      decoration: BoxDecoration(
                        borderRadius: Styles.borderRadius,
                      ),
                      child: OutlinedButton(
                        onPressed: () async {
                          final now = DateTime.now();

                          final result = await showDatePicker(
                              context: context,
                              initialDate: now,
                              firstDate: now.subtract(
                                const Duration(
                                  days: 90,
                                ),
                              ),
                              lastDate: now);

                          if (result != null) {
                            setState(() {
                              selectedDate = result;
                            });
                          }
                        },
                        child: Row(
                          mainAxisAlignment: MainAxisAlignment.spaceBetween,
                          children: [
                            Padding(
                              padding: const EdgeInsets.only(right: 16.0),
                              child:
                                  Text('${formatDateToString(selectedDate)}'),
                            ),
                            Icon(Icons.calendar_today_outlined),
                          ],
                        ),
                      ),
                    ),
                  ],
                ),
              )

            ],
          ),
        );
      },
    );
  }
```

需要注意的是，我已经在我们的主页中添加了 selectedDatevariable，你可以在我最后提供的存储库链接中看到这一点。但是现在我们遇到了一个问题，尽管我们正在使用 setstateoutlinebutton 更新 selectedDate 的值，但是在重新打开工作表之前，仍然会显示旧的值，如下所示。

![](2021-05-18-08-59-31.png)

为了解决这个问题，我们需要将 OutlinedButton 传递给 StatefulBuilder (或者您可以创建一个新的小部件并使用回调公开更改，正如我前面所说的，顺便说一下，这是更正确的方法)。

```dart
void addWeight() {
    showModalBottomSheet(
      isScrollControlled: true,
      context: context,
      shape: const RoundedRectangleBorder(
          borderRadius: BorderRadius.only(
        topLeft: Radius.circular(8),
        topRight: Radius.circular(8),
      )),
      builder: (context) {
        return Container(
          height: 360,
          width: MediaQuery.of(context).size.width,
          margin:
              EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom),
          padding: const EdgeInsets.all(16.0),
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(20),
          ),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              const Padding(
                padding: EdgeInsets.only(bottom: 24.0),
                child: Text(
                  'Register Weight',
                  style: Styles.titleStyle,
                ),
              ),
              TextField(
                controller: weightInputController,
                keyboardType: TextInputType.number,
                decoration: InputDecoration(
                  labelText: 'Weight (KG)',
                  border: OutlineInputBorder(
                    borderRadius: Styles.borderRadius,
                  ),
                ),
              ),
              Padding(
                padding: const EdgeInsets.symmetric(vertical: 8.0),
                child: Row(
                  children: [
                    Expanded(
                      child: Text(
                        'Select a date',
                        style: TextStyle(
                          fontSize: 14,
                          fontWeight: FontWeight.w500,
                        ),
                      ),
                    ),
                    Container(
                        padding: const EdgeInsets.symmetric(horizontal: 4),
                        margin: const EdgeInsets.symmetric(vertical: 8.0),
                        height: 36,
                        decoration: BoxDecoration(
                          borderRadius: Styles.borderRadius,
                        ),
                        child: StatefulBuilder(
                          builder: (context, setState) {
                            return OutlinedButton(
                              onPressed: () async {
                                final now = DateTime.now();

                                final result = await showDatePicker(
                                    context: context,
                                    initialDate: now,
                                    firstDate: now.subtract(
                                      const Duration(
                                        days: 90,
                                      ),
                                    ),
                                    lastDate: now);

                                if (result != null) {
                                  setState(() {
                                    selectedDate = result;
                                  });
                                }
                              },
                              child: Row(
                                mainAxisAlignment:
                                    MainAxisAlignment.spaceBetween,
                                children: [
                                  Padding(
                                    padding: const EdgeInsets.only(right: 16.0),
                                    child: Text(
                                        '${formatDateToString(selectedDate)}'),
                                  ),
                                  Icon(Icons.calendar_today_outlined),
                                ],
                              ),
                            );
                          },
                        )),
                  ],
                ),
              ),
              Expanded(child: Container()),
              ButtonBar(
                children: [
                  ElevatedButton(
                    onPressed: () => Navigator.pop(context),
                    child: Text('Cancel',
                        style: TextStyle(
                          color: Theme.of(context).primaryColor,
                        )),
                    style: ElevatedButton.styleFrom(
                      primary: Colors.white,
                      // minimumSize: Size(96, 48),
                    ),
                  ),
                  ElevatedButton(
                      onPressed: () {
                        setState(() {
                          weights.insert(
                              0,
                              WeightModel(
                                value: double.parse(weightInputController.text),
                                date: selectedDate,
                              ));
                        });
                        Navigator.pop(context);
                      },
                      child: const Text('Register')),
                ],
              ),
            ],
          ),
        );
      },
    );
  }
```

这是我们的 ModalBottomSheet 的最终版本！

https://github.com/evandrmb/bottom_sheet

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
