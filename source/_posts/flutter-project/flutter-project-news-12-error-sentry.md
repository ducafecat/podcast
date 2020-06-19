---
title: Flutter 实战从零开始 新闻客户端 - 12 采用 sentry 平台收集错误
date: 2020-06-05 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

# 本节目标

- 使用 sentry 平台
- flutter 集成
- android 集成
- ios 集成

## 正文

### 错误收集策略

![](flutter-error.png)

### sentry 平台

https://sentry.io

### 收集 flutter

- 参考

https://docs.sentry.io/platforms/flutter/

- pubspec.yaml

```yaml
dependencies:
  sentry: ^3.0.1
```

- lib/main.dart

```dart
// 创建 SentryClient 用于将异常日志上报给 sentry 平台
final SentryClient _sentry = new SentryClient(
  dsn:
      'https://xxxxxxxxxx',
);

// 是否开发环境
bool get isInDebugMode {
  return false; // false 开始上传 sentry
}

// 上报异常的函数
Future<void> _reportError(dynamic error, dynamic stackTrace) async {
  print('Caught error: $error');
  if (isInDebugMode) {
    print(stackTrace);
  } else {
    final SentryResponse response = await _sentry.captureException(
      exception: error,
      stackTrace: stackTrace,
    );

    if (response.isSuccessful) {
      print('Success! Event ID: ${response.eventId}');
    } else {
      print('Failed to report to Sentry.io: ${response.error}');
    }
  }
}

Future<Null> main() async {
  // 捕获并上报 Flutter 异常
  FlutterError.onError = (FlutterErrorDetails details) async {
    if (isInDebugMode == true) {
      FlutterError.dumpErrorToConsole(details);
    } else {
      Zone.current.handleUncaughtError(details.exception, details.stack);
    }
  };

  // 捕获并上报 Dart 异常
  runZonedGuarded(() async {
    await Global.init();
    runApp(
      MultiProvider(
        providers: [
          ChangeNotifierProvider<AppState>.value(
            value: Global.appState,
          ),
        ],
        child: Consumer<AppState>(builder: (context, appState, _) {
          if (appState.isGrayFilter) {
            return ColorFiltered(
              colorFilter: ColorFilter.mode(Colors.white, BlendMode.color),
              child: NewsApp(),
            );
          } else {
            return NewsApp();
          }
        }),
      ),
    );
  }, (Object error, StackTrace stack) {
    _reportError(error, stack);
  });
}
```

### 收集 android

- 参考

https://docs.sentry.io/platforms/android/

- 集成 sdk

```
// ADD JCENTER REPOSITORY
repositories {
    jcenter()
}

// ADD COMPATIBILITY OPTIONS TO BE COMPATIBLE WITH JAVA 1.8
android {
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
}

// ADD SENTRY ANDROID AS A DEPENDENCY
dependencies {
    // https://github.com/getsentry/sentry-android/releases
    implementation 'io.sentry:sentry-android:{version}'
}
```

- android/app/src/main/AndroidManifest.xml

```xml
    <application
        android:name="io.flutter.app.FlutterApplication"
        android:label="猫哥新闻"
        android:icon="@mipmap/launcher_icon">

        ...

        <!-- sentry -->
        <meta-data android:name="io.sentry.dsn" android:value="xxxxxxxxxxxxxxxxx" />
    </application>
```

- android/app/src/main/kotlin/com/example/flutterducafecatnews/CrashHandler.java

```java
public class CrashHandler implements UncaughtExceptionHandler {

    @Override
    public void uncaughtException(Thread t, Throwable e) {
        Sentry.captureException(e);
    }
}
```

- android/app/src/main/kotlin/com/example/flutterducafecatnews/MainActivity.kt

```java
import io.sentry.core.Sentry

class MainActivity: FlutterActivity() {
    override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
        val crashHandler = CrashHandler()
        Thread.setDefaultUncaughtExceptionHandler(crashHandler)
        GeneratedPluginRegistrant.registerWith(flutterEngine)
    }
}
```

### 收集 ios

- 资料

https://docs.sentry.io/platforms/cocoa/?_ga=2.17974013.534595501.1591172359-228174411.1591172359&_gac=1.12380800.1591172359.EAIaIQobChMIrICd9Jrl6QIVCj5gCh2zFw8lEAAYASAAEgJwyfD_BwE&platform=javascript

- 集成 CocoaPods

```
platform :ios, '8.0'
use_frameworks! # This is important

target 'YourApp' do
    pod 'Sentry', :git => 'https://github.com/getsentry/sentry-cocoa.git', :tag => '5.1.2'
end
```

- ios/Runner/AppDelegate.swift

```swift
{

    SentrySDK.start(options: [
        "dsn": "https://xxxxxxxxxxxxxxxxxxx",
        "debug": true, // Enabled debug when first installing is always helpful
        "enableAutoSessionTracking": true
    ])

    NSSetUncaughtExceptionHandler { exception in
     print(exception)
     SentrySDK.capture(message: exception.description)
     SentrySDK.capture(exception: exception)
    }

    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)

  }
```

## 资源

### 参考

- https://docs.sentry.io/platforms/flutter/

- https://docs.sentry.io/platforms/android/

- https://docs.sentry.io/platforms/cocoa/?_ga=2.17974013.534595501.1591172359-228174411.1591172359&_gac=1.12380800.1591172359.EAIaIQobChMIrICd9Jrl6QIVCj5gCh2zFw8lEAAYASAAEgJwyfD_BwE&platform=javascript

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.12

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
