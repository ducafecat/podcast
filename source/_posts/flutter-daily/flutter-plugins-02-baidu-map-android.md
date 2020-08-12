---
title: Flutter 混合开发 - 02 百度地图定位功能 android 篇
date: 2020-08-04 00:00:00
tags: flutter
categories: Flutter混合开发
---

![](2020-08-06-18-10-40.png)

# 本节目标

- 百度地图业务
- 百度组件初始
- 编写定位代码 android 篇

![](2020-08-12-15-01-43.png)

## 环境

```sh
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.20.1, on Mac OS X 10.15.6 19G73, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.2)
[✓] Xcode - develop for iOS and macOS (Xcode 11.6)
[✓] Android Studio (version 4.0)
[✓] VS Code (version 1.47.3)
```

## 视频

## 代码

https://github.com/ducafecat/flutter_baidu_plugin_ducafecat/releases/tag/v1.0.2

可以直接用 👇 v1.0.3

https://github.com/ducafecat/flutter_baidu_plugin_ducafecat/releases/tag/v1.0.3

## 正文

### 创建组件的几种方式

#### 现成轮子直接用

- 官方仓库搜索

  https://pub.dev/flutter/packages

  https://pub.flutter-io.cn/flutter/packages

#### 可参考的组件代码

- 通过仓库，查找 github 代码仓

- 网站、客服索取代码

#### 参考官方集成文档编写组件

- 官网文档

http://lbsyun.baidu.com/index.php?title=flutter/loc

### 组件代码

#### 百度应用管理，创建 AK

- 应用管理

https://lbsyun.baidu.com/apiconsole/key#/home

![](2020-08-06-16-27-33.png)

- 查询 SHA1

http://lbsyun.baidu.com/index.php?title=FAQ/SHA1

![](2020-08-06-16-29-26.png)

```sh
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey
```

- 设置 AK

example/android/app/src/main/AndroidManifest.xml

```xml

...

        <!-- 在这里设置android端ak-->
        <meta-data
            android:name="com.baidu.lbsapi.API_KEY"
            android:value="aCUtcLDufllGi4nEaKgU8FmBqufFyekh" />

    </application>
</manifest>

```

#### 设置 Android 权限

- 文档

http://lbsyun.baidu.com/index.php?title=android-locsdk/guide/create-project/android-studio

- android/src/main/AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="tech.ducafecat.flutter_baidu_plugin_ducafecat">
    <!-- 这个权限用于进行网络定位-->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"></uses-permission>
    <!-- 这个权限用于访问GPS定位-->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"></uses-permission>
    <!-- 用于访问wifi网络信息，wifi信息会用于进行网络定位-->
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
    <!-- 获取运营商信息，用于支持提供运营商信息相关的接口-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
    <!-- 这个权限用于获取wifi的获取权限，wifi信息会用来进行网络定位-->
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"></uses-permission>
    <!-- 用于读取手机当前的状态-->
    <uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
    <!-- 写入扩展存储，向扩展卡写入数据，用于写入离线定位数据-->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    <!-- 访问网络，网络定位需要上网-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- 读取系统信息，包含系统版本等信息，用作统计-->
    <uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
    <!-- 程序在手机屏幕关闭后后台进程仍然运行-->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <application>
        <!-- 声明service组件 -->
        <service
            android:name="com.baidu.location.f"
            android:enabled="true"
            android:process=":remote" >
        </service>
    </application>
</manifest>
```

#### 添加 android libs 库文件

- 目录 android/libs

![](2020-08-06-16-27-07.png)

- android/build.gradle

```gradle
...
android {
    compileSdkVersion 28

    sourceSets {
        main {
            jniLibs.srcDir 'libs'
        }
    }

    defaultConfig {
        minSdkVersion 16
    }
    lintOptions {
        disable 'InvalidPackage'
    }
}

dependencies {
    implementation files('libs/BaiduLBS_Android.jar')
}

```

#### 编写 Flutter 组件代码

- 目录 lib

![](2020-08-06-16-30-34.png)

- 地理信息 lib/entity/flutter_baidu_location.dart

```dart
/// 百度定位结果类，用于存储各类定位结果信息
class BaiduLocation {
  /// 定位成功时间
  final String locTime;

  /// 定位结果类型
  final int locType;

  /// 半径
  final double radius;

  /// 纬度
  final double latitude;

  /// 经度
  final double longitude;

  /// 海拔
  final double altitude;

  /// 国家
  final String country;

  /// 省份
  final String province;

  /// 城市
  final String city;

  /// 区县
  final String district;

  /// 街道
  final String street;

  /// 地址
  final String address;

  /// 位置语义化描述，例如"在百度大厦附近"
  final String locationDetail;

  /// 周边poi信息，每个poi之间用"|"隔开

  final String poiList;

  /// 定位结果回调时间
  final String callbackTime;

  /// 错误码
  final int errorCode;

  /// 定位失败描述信息
  final String errorInfo;

  BaiduLocation(
      {this.locTime,
      this.locType,
      this.radius,
      this.latitude,
      this.longitude,
      this.altitude,
      this.country,
      this.province,
      this.city,
      this.district,
      this.street,
      this.address,
      this.locationDetail,
      this.poiList,
      this.callbackTime,
      this.errorCode,
      this.errorInfo});

  /// 根据传入的map生成BaiduLocation对象
  factory BaiduLocation.fromMap(dynamic value) {
    return new BaiduLocation(
      locTime: value['locTime'],
      locType: value['locType'],
      radius: value['radius'],
      latitude: value['latitude'],
      longitude: value['longitude'],
      altitude: value['altitude'],
      country: value['country'],
      province: value['province'],
      city: value['city'],
      district: value['district'],
      street: value['street'],
      address: value['address'],
      locationDetail: value['locationDetail'],
      poiList: value['poiList'],
      callbackTime: value['callbackTime'],
      errorCode: value['errorCode'],
      errorInfo: value['errorInfo'],
    );
  }

  /// 获取对本类所有变量赋值后的map键值对
  Map getMap() {
    return {
      "locTime": locTime,
      "locType": locType,
      "radius": radius,
      "latitude": latitude,
      "longitude": longitude,
      "altitude": altitude,
      "country": country,
      "province": province,
      "city": city,
      "district": district,
      "street": street,
      "address": address,
      "locationDescribe": locationDetail,
      "poiList": poiList,
      "callbackTime": callbackTime,
      "errorCode": errorCode,
      "errorInfo": errorInfo,
    };
  }
}

```

- android 配置项 lib/entity/flutter_baidu_location_android_option.dart

```dart
/// 设置android端定位参数类
class BaiduLocationAndroidOption {
  /// 坐标系类型
  String coorType;

  /// 是否需要返回地址信息
  bool isNeedAddres;

  /// 是否需要返回海拔高度信息
  bool isNeedAltitude;

  /// 是否需要返回周边poi信息
  bool isNeedLocationPoiList;

  /// 是否需要返回新版本rgc信息
  bool isNeedNewVersionRgc;

  /// 是否需要返回位置描述信息
  bool isNeedLocationDescribe;

  /// 是否使用gps
  bool openGps;

  /// 可选，设置发起定位请求的间隔，int类型，单位ms
  /// 如果设置为0，则代表单次定位，即仅定位一次，默认为0
  /// 如果设置非0，需设置1000ms以上才有效
  int scanspan;

  /// 设置定位模式，可选的模式有高精度、仅设备、仅网络。默认为高精度模式
  int locationMode;

  /// 可选，设置场景定位参数，包括签到场景、运动场景、出行场景
  int locationPurpose;

  /// 可选，设置返回经纬度坐标类型，默认GCJ02
  /// GCJ02：国测局坐标；
  /// BD09ll：百度经纬度坐标；
  /// BD09：百度墨卡托坐标；
  /// 海外地区定位，无需设置坐标类型，统一返回WGS84类型坐标
  void setCoorType(String coorType) {
    this.coorType = coorType;
  }

  /// 是否需要返回地址信息
  void setIsNeedAddres(bool isNeedAddres) {
    this.isNeedAddres = isNeedAddres;
  }

  /// 是否需要返回海拔高度信息
  void setIsNeedAltitude(bool isNeedAltitude) {
    this.isNeedAltitude = isNeedAltitude;
  }

  /// 是否需要返回周边poi信息
  void setIsNeedLocationPoiList(bool isNeedLocationPoiList) {
    this.isNeedLocationPoiList = isNeedLocationPoiList;
  }

  /// 是否需要返回位置描述信息
  void setIsNeedLocationDescribe(bool isNeedLocationDescribe) {
    this.isNeedLocationDescribe = isNeedLocationDescribe;
  }

  /// 是否需要返回新版本rgc信息
  void setIsNeedNewVersionRgc(bool isNeedNewVersionRgc) {
    this.isNeedNewVersionRgc = isNeedNewVersionRgc;
  }

  /// 是否使用gps
  void setOpenGps(bool openGps) {
    this.openGps = openGps;
  }

  /// 可选，设置发起定位请求的间隔，int类型，单位ms
  /// 如果设置为0，则代表单次定位，即仅定位一次，默认为0
  /// 如果设置非0，需设置1000ms以上才有效
  void setScanspan(int scanspan) {
    this.scanspan = scanspan;
  }

  /// 设置定位模式，可选的模式有高精度、仅设备、仅网络，默认为高精度模式
  void setLocationMode(LocationMode locationMode) {
    if (locationMode == LocationMode.Hight_Accuracy) {
      this.locationMode = 1;
    } else if (locationMode == LocationMode.Device_Sensors) {
      this.locationMode = 2;
    } else if (locationMode == LocationMode.Battery_Saving) {
      this.locationMode = 3;
    }
  }

  /// 可选，设置场景定位参数，包括签到场景、运动场景、出行场景
  void setLocationPurpose(BDLocationPurpose locationPurpose) {
    if (locationPurpose == BDLocationPurpose.SignIn) {
      this.locationPurpose = 1;
    } else if (locationPurpose == BDLocationPurpose.Transport) {
      this.locationPurpose = 2;
    } else if (locationPurpose == BDLocationPurpose.Sport) {
      this.locationPurpose = 3;
    }
  }

  BaiduLocationAndroidOption(
      {this.coorType,
      this.isNeedAddres,
      this.isNeedAltitude,
      this.isNeedLocationPoiList,
      this.isNeedNewVersionRgc,
      this.openGps,
      this.isNeedLocationDescribe,
      this.scanspan,
      this.locationMode,
      this.locationPurpose});

  /// 根据传入的map生成BaiduLocationAndroidOption对象
  factory BaiduLocationAndroidOption.fromMap(dynamic value) {
    return new BaiduLocationAndroidOption(
      coorType: value['coorType'],
      isNeedAddres: value['isNeedAddres'],
      isNeedAltitude: value['isNeedAltitude'],
      isNeedLocationPoiList: value['isNeedLocationPoiList'],
      isNeedNewVersionRgc: value['isNeedNewVersionRgc'],
      openGps: value['openGps'],
      isNeedLocationDescribe: value[''],
      scanspan: value['scanspan'],
      locationMode: value['locationMode'],
      locationPurpose: value['LocationPurpose'],
    );
  }

  /// 获取对本类所有变量赋值后的map键值对
  Map getMap() {
    return {
      "coorType": coorType,
      "isNeedAddres": isNeedAddres,
      "isNeedAltitude": isNeedAltitude,
      "isNeedLocationPoiList": isNeedLocationPoiList,
      "isNeedNewVersionRgc": isNeedNewVersionRgc,
      "openGps": openGps,
      "isNeedLocationDescribe": isNeedLocationDescribe,
      "scanspan": scanspan,
      "locationMode": locationMode,
    };
  }
}

/// 定位模式枚举类
enum LocationMode {
  /// 高精度模式
  Hight_Accuracy,

  /// 低功耗模式
  Battery_Saving,

  /// 仅设备(Gps)模式
  Device_Sensors
}

/// 场景定位枚举类
enum BDLocationPurpose {
  ///  签到场景
  /// 只进行一次定位返回最接近真实位置的定位结果（定位速度可能会延迟1-3s）
  SignIn,

  /// 出行场景
  /// 高精度连续定位，适用于有户内外切换的场景，卫星定位和网络定位相互切换，卫星定位成功之后网络定位不再返回，卫星信号断开之后一段时间才会返回网络结果
  Sport,

  /// 运动场景
  /// 高精度连续定位，适用于有户内外切换的场景，卫星定位和网络定位相互切换，卫星定位成功之后网络定位不再返回，卫星信号断开之后一段时间才会返回网络结果
  Transport
}

```

- ios 配置项 lib/entity/flutter_baidu_location_ios_option.dart

```dart
/// 设置ios端定位参数类
class BaiduLocationIOSOption {
  /// 设置位置获取超时时间
  int locationTimeout;

  /// 设置获取地址信息超时时间
  int reGeocodeTimeout;

  /// 设置应用位置类型
  String activityType;

  /// 设置返回位置的坐标系类型
  String BMKLocationCoordinateType;

  /// 设置预期精度参数
  String desiredAccuracy;

  /// 是否需要最新版本rgc数据
  bool isNeedNewVersionRgc;

  /// 指定定位是否会被系统自动暂停
  bool pausesLocationUpdatesAutomatically;

  /// 指定是否允许后台定位
  bool allowsBackgroundLocationUpdates;

  /// 设定定位的最小更新距离
  double distanceFilter;

  /// 指定是否允许后台定位
  /// allowsBackgroundLocationUpdates为true则允许后台定位
  /// allowsBackgroundLocationUpdates为false则不允许后台定位
  void setAllowsBackgroundLocationUpdates(
      bool allowsBackgroundLocationUpdates) {
    this.allowsBackgroundLocationUpdates = allowsBackgroundLocationUpdates;
  }

  /// 指定定位是否会被系统自动暂停
  /// pausesLocationUpdatesAutomatically为true则定位会被系统自动暂停
  /// pausesLocationUpdatesAutomatically为false则定位不会被系统自动暂停
  void setPauseLocUpdateAutomatically(bool pausesLocationUpdatesAutomatically) {
    this.pausesLocationUpdatesAutomatically =
        pausesLocationUpdatesAutomatically;
  }

  /// 设置位置获取超时时间
  void setLocationTimeout(int locationTimeout) {
    this.locationTimeout = locationTimeout;
  }

  /// 设置获取地址信息超时时间
  void setReGeocodeTimeout(int reGeocodeTimeout) {
    this.reGeocodeTimeout = reGeocodeTimeout;
  }

  /// 设置应用位置类型
  /// activityType可选值包括:
  /// "CLActivityTypeOther"
  /// "CLActivityTypeAutomotiveNavigation"
  /// "CLActivityTypeFitness"
  /// "CLActivityTypeOtherNavigation"
  void setActivityType(String activityType) {
    this.activityType = activityType;
  }

  /// 设置返回位置的坐标系类型
  /// BMKLocationCoordinateType可选值包括:
  /// "BMKLocationCoordinateTypeBMK09LL"
  /// "BMKLocationCoordinateTypeBMK09MC"
  /// "BMKLocationCoordinateTypeWGS84"
  /// "BMKLocationCoordinateTypeGCJ02"
  void setBMKLocationCoordinateType(String BMKLocationCoordinateType) {
    this.BMKLocationCoordinateType = BMKLocationCoordinateType;
  }

  /// 设置预期精度参数
  /// desiredAccuracy可选值包括:
  /// "kCLLocationAccuracyBest"
  /// "kCLLocationAccuracyNearestTenMeters"
  /// "kCLLocationAccuracyHundredMeters"
  /// "kCLLocationAccuracyKilometer"
  void setDesiredAccuracy(String desiredAccuracy) {
    this.desiredAccuracy = desiredAccuracy;
  }

  /// 设定定位的最小更新距离
  void setDistanceFilter(double distanceFilter) {
    this.distanceFilter = distanceFilter;
  }

  /// 是否需要最新版本rgc数据
  /// isNeedNewVersionRgc为true则需要返回最新版本rgc数据
  /// isNeedNewVersionRgc为false则不需要返回最新版本rgc数据
  void setIsNeedNewVersionRgc(bool isNeedNewVersionRgc) {
    this.isNeedNewVersionRgc = isNeedNewVersionRgc;
  }

  BaiduLocationIOSOption(
      {this.locationTimeout,
      this.reGeocodeTimeout,
      this.activityType,
      this.BMKLocationCoordinateType,
      this.desiredAccuracy,
      this.isNeedNewVersionRgc,
      this.pausesLocationUpdatesAutomatically,
      this.allowsBackgroundLocationUpdates,
      this.distanceFilter});

  /// 根据传入的map生成BaiduLocationIOSOption对象
  factory BaiduLocationIOSOption.fromMap(dynamic value) {
    return new BaiduLocationIOSOption(
      locationTimeout: value['locationTimeout'],
      reGeocodeTimeout: value['reGeocodeTimeout'],
      activityType: value['activityType'],
      BMKLocationCoordinateType: value['BMKLocationCoordinateType'],
      desiredAccuracy: value['desiredAccuracy'],
      isNeedNewVersionRgc: value['isNeedNewVersionRgc'],
      pausesLocationUpdatesAutomatically:
          value['pausesLocationUpdatesAutomatically'],
      allowsBackgroundLocationUpdates: value['allowsBackgroundLocationUpdates'],
      distanceFilter: value['distanceFilter'],
    );
  }

  /// 获取对本类所有变量赋值后的map键值对
  Map getMap() {
    return {
      "locationTimeout": locationTimeout,
      "reGeocodeTimeout": reGeocodeTimeout,
      "activityType": activityType,
      "BMKLocationCoordinateType": BMKLocationCoordinateType,
      "desiredAccuracy": desiredAccuracy,
      "isNeedNewVersionRgc": isNeedNewVersionRgc,
      "pausesLocationUpdatesAutomatically": pausesLocationUpdatesAutomatically,
      "allowsBackgroundLocationUpdates": allowsBackgroundLocationUpdates,
      "distanceFilter": distanceFilter,
    };
  }
}

```

- 接口 lib/flutter_baidu_plugin_ducafecat.dart

```dart
import 'dart:async';
import 'dart:io';
import 'package:flutter/services.dart';

class FlutterBaiduPluginDucafecat {
  /// flutter端主动调用原生端方法
  static const MethodChannel _channel =
      const MethodChannel('flutter_baidu_plugin_ducafecat');

  /// 原生端主动回传结果数据到flutter端
  static const EventChannel _stream =
      const EventChannel("flutter_baidu_plugin_ducafecat_stream");

  /// ios 下设置 key
  /// android 在 AndroidManifest.xml 中设置
  static Future<bool> setApiKey(String key) async {
    return await _channel.invokeMethod("setApiKey", key);
  }

  /// 设置定位参数
  void prepareLoc(Map androidMap, Map iosMap) {
    Map map;
    if (Platform.isAndroid) {
      map = androidMap;
    } else {
      map = iosMap;
    }
    _channel.invokeMethod("updateOption", map);
    return;
  }

  /// 启动定位
  void startLocation() {
    _channel.invokeMethod('startLocation');
    return;
  }

  /// 停止定位
  void stopLocation() {
    _channel.invokeMethod('stopLocation');
    return;
  }

  /// 原生端回传键值对map到flutter端
  /// map中key为isInChina对应的value，如果为1则判断是在国内，为0则判断是在国外
  /// map中存在key为nearby则判断为已到达设置监听位置附近
  Stream<Map<String, Object>> onResultCallback() {
    Stream<Map<String, Object>> _resultMap;
    if (_resultMap == null) {
      _resultMap = _stream.receiveBroadcastStream().map<Map<String, Object>>(
          (element) => element.cast<String, Object>());
    }
    return _resultMap;
  }
}

```

#### 触发 registerWith 的方式，老项目

> // This static function is optional and equivalent to onAttachedToEngine. It supports the old
> // pre-Flutter-1.12 Android projects. You are encouraged to continue supporting
> // plugin registration via this function while apps migrate to use the new Android APIs
> // post-flutter-1.12 via https://flutter.dev/go/android-project-migration.

- android 组件代码

android/src/main/java/tech/ducafecat/flutter_baidu_plugin_ducafecat/FlutterBaiduPluginDucafecatPlugin.java

```java
  public static void registerWith(Registrar registrar) {
    ......
  }
```

- example android 注册组件

example/android/app/src/main/java/io/flutter/plugins/GeneratedPluginRegistrant.java

```java
public final class GeneratedPluginRegistrant {
  public static void registerWith(PluginRegistry registry) {
    if (alreadyRegisteredWith(registry)) {
      return;
    }
    LocationFlutterPlugin.registerWith(registry.registrarFor("com.baidu.bdmap_location_flutter_plugin.LocationFlutterPlugin"));
    PermissionHandlerPlugin.registerWith(registry.registrarFor("com.baseflow.permissionhandler.PermissionHandlerPlugin"));
  }

  private static boolean alreadyRegisteredWith(PluginRegistry registry) {
    final String key = GeneratedPluginRegistrant.class.getCanonicalName();
    if (registry.hasPlugin(key)) {
      return true;
    }
    registry.registrarFor(key);
    return false;
  }
}
```

#### 成员变量、同步、异步处理

MethodChannel 请求方法后，同步返回结果

EventChannel 组件主动推消息到 Flutter

- android/src/main/java/tech/ducafecat/flutter_baidu_plugin_ducafecat/FlutterBaiduPluginDucafecatPlugin.java

用到的成员变量先定义下

```java
public class FlutterBaiduPluginDucafecatPlugin implements FlutterPlugin, MethodCallHandler, EventChannel.StreamHandler {

  // 通道名称
  private static final String CHANNEL_METHOD_LOCATION = "flutter_baidu_plugin_ducafecat";
  private static final String CHANNEL_STREAM_LOCATION = "flutter_baidu_plugin_ducafecat_stream";

  private Context mContext = null; // flutter view context
  private LocationClient mLocationClient = null; // 定位对象
  private EventChannel.EventSink mEventSink = null; // 事件对象
  private BDNotifyListener mNotifyListener; // 位置提醒对象

  private boolean isPurporseLoc = false; // 签到场景
  private boolean isInChina = false;  // 是否启用国内外位置判断功能
  private boolean isNotify = false; // 位置提醒

  // 通道对象
  private MethodChannel channel = null;
  private EventChannel eventChannel = null;

```

#### 组件生命周期

- 文件

android/src/main/java/tech/ducafecat/flutter_baidu_plugin_ducafecat/FlutterBaiduPluginDucafecatPlugin.java

- 组件注册 onAttachedToEngine

```java
  @Override
  public void onAttachedToEngine(@NonNull FlutterPluginBinding flutterPluginBinding) {

    this.mContext = flutterPluginBinding.getApplicationContext();

    /**
     * 开始、停止定位
     */
    channel = new MethodChannel(flutterPluginBinding.getBinaryMessenger(), CHANNEL_METHOD_LOCATION);
    channel.setMethodCallHandler(this);

    /**
     * 监听位置变化
     */
    eventChannel = new EventChannel(flutterPluginBinding.getBinaryMessenger(), CHANNEL_STREAM_LOCATION);
    eventChannel.setStreamHandler(this);
  }
```

- 老项目 组件注册 registerWith

```java
  public static void registerWith(Registrar registrar) {

    FlutterBaiduPluginDucafecatPlugin plugin = new FlutterBaiduPluginDucafecatPlugin();
    plugin.mContext = registrar.context();

    /**
     * 开始、停止定位
     */
    final MethodChannel channel = new MethodChannel(registrar.messenger(), CHANNEL_METHOD_LOCATION);
    channel.setMethodCallHandler(plugin);

    /**
     * 监听位置变化
     */
    final EventChannel eventChannel = new EventChannel(registrar.messenger(), CHANNEL_STREAM_LOCATION);
    eventChannel.setStreamHandler(plugin);

//    final MethodChannel channel = new MethodChannel(registrar.messenger(), "flutter_baidu_plugin_ducafecat");
//    channel.setMethodCallHandler(new FlutterBaiduPluginDucafecatPlugin());
  }
```

- 注销组件 onCancel

```java
  @Override
  public void onCancel(Object arguments) {
    stopLocation();
    if (isNotify) {
      if (null != mLocationClient) {
        mLocationClient.removeNotifyEvent(mNotifyListener);
      }
      mNotifyListener = null;
    }
  }
```

- 销毁组件 onDetachedFromEngine

```java
  @Override
  public void onDetachedFromEngine(@NonNull FlutterPluginBinding binding) {
    channel.setMethodCallHandler(null);
    eventChannel.setStreamHandler(null);
  }
```

- 方法调用 onMethodCall

```java
  @Override
  public void onMethodCall(@NonNull MethodCall call, @NonNull Result result) {
    if ("startLocation".equals(call.method)) {
      startLocation(); // 启动定位
    } else if ("stopLocation".equals(call.method)) {
      stopLocation(); // 停止定位
    } else if("updateOption".equals(call.method)) { // 设置定位参数
      try {
        updateOption((Map) call.arguments);
      } catch (Exception e) {
        e.printStackTrace();
      }
    } else if (("getPlatformVersion").equals(call.method)) {
      result.success("Android " + android.os.Build.VERSION.RELEASE);
    } else {
      result.notImplemented();
    }
  }
```

- flutter onListen 回调对象

```java
  @Override
  public void onListen(Object arguments, EventChannel.EventSink events) {
    mEventSink = events;
  }
```

#### 地图业务参数

- 更新参数 updateOption

```java
  /**
   * 准备定位
   * @param arguments
   */
  private void updateOption(Map arguments) {
    if (null == mLocationClient) {
      mLocationClient = new LocationClient(mContext);
    }
    // 判断是否启用位置提醒功能
    if (arguments.containsKey("isNotify")) {
      isNotify = true;
      if (null == mNotifyListener) {
        mNotifyListener = new MyNotifyLister();
      }
      mLocationClient.registerNotify(mNotifyListener);
      double lat = 0;
      double lon = 0;
      float radius = 0;

      if (arguments.containsKey("latitude")) {
        lat = (double)arguments.get("latitude");
      }

      if (arguments.containsKey("longitude")) {
        lon = (double)arguments.get("longitude");
      }

      if (arguments.containsKey("radius")) {
        double radius1 = (double)arguments.get("radius");
        radius = Float.parseFloat(String.valueOf(radius1));
      }

      String coorType = mLocationClient.getLocOption().getCoorType();
      mNotifyListener.SetNotifyLocation(lat, lon, radius, coorType);
      return;
    } else {
      isNotify = false;
    }

    mLocationClient.registerLocationListener(new CurrentLocationListener());

    // 判断是否启用国内外位置判断功能
    if (arguments.containsKey("isInChina")) {
      isInChina = true;
      return;
    } else {
      isInChina =false;
    }


    LocationClientOption option = new LocationClientOption();
    parseOptions(option, arguments);
    option.setProdName("flutter");
    mLocationClient.setLocOption(option);
  }
```

- 解析定位参数 parseOptions

```java
  /**
   * 解析定位参数
   * @param option
   * @param arguments
   */
  private void parseOptions(LocationClientOption option,Map arguments) {
    if (arguments != null) {

      // 可选，设置是否返回逆地理地址信息。默认是true
      if (arguments.containsKey("isNeedAddres")) {
        if (((boolean)arguments.get("isNeedAddres"))) {
          option.setIsNeedAddress(true);
        } else {
          option.setIsNeedAddress(false);
        }
      }

      // 可选，设置定位模式，可选的模式有高精度、仅设备、仅网络。默认为高精度模式
      if (arguments.containsKey("locationMode")) {
        if (((int)arguments.get("locationMode")) == 1) {
          option.setLocationMode(LocationClientOption.LocationMode.Hight_Accuracy); // 高精度模式
        } else if (((int)arguments.get("locationMode")) == 2) {
          option.setLocationMode(LocationClientOption.LocationMode.Device_Sensors); // 仅设备模式
        } else if (((int)arguments.get("locationMode")) == 3) {
          option.setLocationMode(LocationClientOption.LocationMode.Battery_Saving); // 仅网络模式
        }
      }

      // 可选，设置场景定位参数，包括签到场景、运动场景、出行场景
      if ((arguments.containsKey("LocationPurpose"))) {
        isPurporseLoc = true;
        if  (((int)arguments.get("LocationPurpose")) == 1) {
          option.setLocationPurpose(LocationClientOption.BDLocationPurpose.SignIn); // 签到场景
        } else if (((int)arguments.get("LocationPurpose")) == 2) {
          option.setLocationPurpose(LocationClientOption.BDLocationPurpose.Transport); // 运动场景
        } else if (((int)arguments.get("LocationPurpose")) == 3) {
          option.setLocationPurpose(LocationClientOption.BDLocationPurpose.Sport); // 出行场景
        }
      } else {
        isPurporseLoc = false;
      }

      // 可选，设置需要返回海拔高度信息
      if (arguments.containsKey("isNeedAltitude")) {
        if (((boolean)arguments.get("isNeedAltitude"))) {
          option.setIsNeedAddress(true);
        } else {
          option.setIsNeedAltitude(false);
        }
      }

      // 可选，设置是否使用gps，默认false
      if (arguments.containsKey("openGps")) {
        if(((boolean)arguments.get("openGps"))) {
          option.setOpenGps(true);
        } else {
          option.setOpenGps(false);
        }
      }

      // 可选，设置是否允许返回逆地理地址信息，默认是true
      if (arguments.containsKey("isNeedLocationDescribe")) {
        if(((boolean)arguments.get("isNeedLocationDescribe"))) {
          option.setIsNeedLocationDescribe(true);
        } else {
          option.setIsNeedLocationDescribe(false);
        }
      }

      // 可选，设置发起定位请求的间隔，int类型，单位ms
      // 如果设置为0，则代表单次定位，即仅定位一次，默认为0
      // 如果设置非0，需设置1000ms以上才有效
      if (arguments.containsKey("scanspan")) {
        option.setScanSpan((int)arguments.get("scanspan"));
      }
      // 可选，设置返回经纬度坐标类型，默认GCJ02
      // GCJ02：国测局坐标；
      // BD09ll：百度经纬度坐标；
      // BD09：百度墨卡托坐标；
      // 海外地区定位，无需设置坐标类型，统一返回WGS84类型坐标
      if (arguments.containsKey("coorType")) {
        option.setCoorType((String)arguments.get("coorType"));
      }

      // 设置是否需要返回附近的poi列表
      if (arguments.containsKey("isNeedLocationPoiList")) {
        if (((boolean)arguments.get("isNeedLocationPoiList"))) {
          option.setIsNeedLocationPoiList(true);
        } else {
          option.setIsNeedLocationPoiList(false);
        }
      }
      // 设置是否需要最新版本rgc数据
      if (arguments.containsKey("isNeedNewVersionRgc")) {
        if (((boolean)arguments.get("isNeedNewVersionRgc"))) {
          option.setIsNeedLocationPoiList(true);
        } else {
          option.setIsNeedLocationPoiList(false);
        }
      }
    }
  }
```

#### 编写启动、停止功能

- 开始定位

```java

  private void startLocation() {
    if(null != mLocationClient) {
      mLocationClient.start();
    }
  }

```

- 停止定位

```java

  private void stopLocation() {
    if (null != mLocationClient) {
      mLocationClient.stop();
      mLocationClient = null;
    }
  }

```

#### 百度定位回调

- CurrentLocationListener

```java

  /**
   * 格式化时间
   *
   * @param time
   * @param strPattern
   * @return
   */
  private String formatUTC(long time, String strPattern) {
    if (TextUtils.isEmpty(strPattern)) {
      strPattern = "yyyy-MM-dd HH:mm:ss";
    }
    SimpleDateFormat sdf = null;
    try {
      sdf = new SimpleDateFormat(strPattern, Locale.CHINA);
      sdf.applyPattern(strPattern);
    } catch (Throwable e) {
      e.printStackTrace();
    }
    return sdf == null ? "NULL" : sdf.format(time);
  }

  class CurrentLocationListener extends BDAbstractLocationListener {

    @Override
    public void onReceiveLocation(BDLocation bdLocation) {

      if (null == mEventSink) {
        return;
      }

      Map<String, Object> result = new LinkedHashMap<>();

      // 判断国内外获取结果
      if (isInChina) {
        if (bdLocation.getLocationWhere() == BDLocation.LOCATION_WHERE_IN_CN) {
          result.put("isInChina", 1); // 在国内
        } else {
          result.put("isInChina", 0); // 在国外
        }
        mEventSink.success(result);
        return;
      }

      // 场景定位获取结果
      if (isPurporseLoc) {
        result.put("latitude", bdLocation.getLatitude()); // 纬度
        result.put("longitude", bdLocation.getLongitude()); // 经度
        mEventSink.success(result);
        return;
      }
      result.put("callbackTime", formatUTC(System.currentTimeMillis(), "yyyy-MM-dd HH:mm:ss"));
      if (null != bdLocation) {
        if (bdLocation.getLocType() == BDLocation.TypeGpsLocation
                || bdLocation.getLocType() == BDLocation.TypeNetWorkLocation
                || bdLocation.getLocType() == BDLocation.TypeOffLineLocation) {
          result.put("locType", bdLocation.getLocType()); // 定位结果类型
          result.put("locTime", bdLocation.getTime()); // 定位成功时间
          result.put("latitude", bdLocation.getLatitude()); // 纬度
          result.put("longitude", bdLocation.getLongitude()); // 经度
          if (bdLocation.hasAltitude()) {
            result.put("altitude", bdLocation.getAltitude()); // 高度
          }
          result.put("radius", Double.parseDouble(String.valueOf(bdLocation.getRadius()))); // 定位精度
          result.put("country", bdLocation.getCountry()); // 国家
          result.put("province", bdLocation.getProvince()); // 省份
          result.put("city", bdLocation.getCity()); // 城市
          result.put("district", bdLocation.getDistrict()); // 区域
          result.put("town", bdLocation.getTown()); // 城镇
          result.put("street", bdLocation.getStreet()); // 街道
          result.put("address", bdLocation.getAddrStr()); // 地址
          result.put("locationDetail", bdLocation.getLocationDescribe()); // 位置语义化描述
          if (null != bdLocation.getPoiList() && !bdLocation.getPoiList().isEmpty()) {

            List<Poi> pois = bdLocation.getPoiList();
            StringBuilder stringBuilder = new StringBuilder();

            if (pois.size() == 1) {
              stringBuilder.append(pois.get(0).getName()).append(",").append(pois.get(0).getTags())
                      .append(pois.get(0).getAddr());
            } else {
              for (int i = 0; i < pois.size() - 1; i++) {
                stringBuilder.append(pois.get(i).getName()).append(",").append(pois.get(i).getTags())
                        .append(pois.get(i).getAddr()).append("|");
              }
              stringBuilder.append(pois.get(pois.size()-1).getName()).append(",").append(pois.get(pois.size()-1).getTags())
                      .append(pois.get(pois.size()-1).getAddr());

            }

            result.put("poiList",stringBuilder.toString()); // 周边poi信息
//
          }
          if (bdLocation.getFloor() != null) {
            // 当前支持高精度室内定位
            String buildingID = bdLocation.getBuildingID();// 百度内部建筑物ID
            String buildingName = bdLocation.getBuildingName();// 百度内部建筑物缩写
            String floor = bdLocation.getFloor();// 室内定位的楼层信息，如 f1,f2,b1,b2
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.append(buildingID).append("-").append(buildingName).append("-").append(floor);
            result.put("indoor", stringBuilder.toString()); // 室内定位结果信息
            mLocationClient.startIndoorMode();// 开启室内定位模式（重复调用也没问题），开启后，定位SDK会融合各种定位信息（GPS,WI-FI，蓝牙，传感器等）连续平滑的输出定位结果；
          } else {
            mLocationClient.stopIndoorMode(); // 处于室外则关闭室内定位模式
          }
        } else {
          result.put("errorCode", bdLocation.getLocType()); // 定位结果错误码
          result.put("errorInfo", bdLocation.getLocTypeDescription()); // 定位失败描述信息
        }
      } else {
        result.put("errorCode", -1);
        result.put("errorInfo", "location is null");
      }
      mEventSink.success(result); // android端实时检测位置变化，将位置结果发送到flutter端
    }
  }
```

#### 位置提醒服务

```java
  public class MyNotifyLister extends BDNotifyListener {
    // 已到达设置监听位置附近
    public void onNotify(BDLocation mlocation, float distance){
      if (null == mEventSink) {
        return;
      }

      Map<String, Object> result = new LinkedHashMap<>();
      result.put("nearby", "已到达设置监听位置附近"); // 1为已经到达 0为未到达
      mEventSink.success(result);
    }
  }
```

### Example 代码

#### 动态授权

- example/pubspec.yaml

```yaml
dependencies:
  flutter:
    sdk: flutter

  ...

  permission_handler: ^5.0.1+1
```

- example/lib/main.dart

```dart

class _MyAppState extends State<MyApp> {
  @override
  void initState() {
    super.initState();
    _requestPermission(); // 执行权限请求
  }

  // 动态申请定位权限
  Future<bool> _requestPermission() async {
    Map<Permission, PermissionStatus> statuses = await [
      Permission.location,
      Permission.storage,
    ].request();

    return statuses[Permission.location].isGranted &&
        statuses[Permission.storage].isGranted;
  }
}
```

#### 主界面代码

- example/lib/main.dart

```dart
import 'dart:io';

import 'package:flutter/material.dart';
import 'dart:async';
import 'package:flutter_baidu_plugin_ducafecat/flutter_baidu_plugin_ducafecat.dart';
import 'package:flutter_baidu_plugin_ducafecat_example/views/location-view.dart';
import 'package:permission_handler/permission_handler.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  void initState() {
    super.initState();
    _requestPermission(); // 执行权限请求

    if (Platform.isIOS == true) {
      FlutterBaiduPluginDucafecat.setApiKeyForIOS(
          "dkYT07blcAj3drBbcN1eGFYqt16HP1pR");
    }
  }

  @override
  void dispose() {
    super.dispose();
  }

  // 动态申请定位权限
  Future<bool> _requestPermission() async {
    Map<Permission, PermissionStatus> statuses = await [
      Permission.location,
      Permission.storage,
    ].request();

    return statuses[Permission.location].isGranted &&
        statuses[Permission.storage].isGranted;
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        "location_view": (context) => LocationView(),
      },
      home: MyHome(),
    );
  }
}

class MyHome extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('地图插件')),
      body: SingleChildScrollView(
        child: Column(
          children: [
            ListTile(
              title: Text('定位信息'),
              subtitle: Text('点击开始后，百度地图实时推送经纬度信息'),
              leading: Icon(Icons.location_searching),
              trailing: Icon(Icons.keyboard_arrow_right),
              onTap: () {
                Navigator.pushNamed(context, "location_view");
              },
            )
          ],
        ),
      ),
    );
  }
}

```

#### 定位服务代码

- example/lib/views/location-view.dart

```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter_baidu_plugin_ducafecat/entity/flutter_baidu_location.dart';
import 'package:flutter_baidu_plugin_ducafecat/entity/flutter_baidu_location_android_option.dart';
import 'package:flutter_baidu_plugin_ducafecat/entity/flutter_baidu_location_ios_option.dart';
import 'package:flutter_baidu_plugin_ducafecat/flutter_baidu_plugin_ducafecat.dart';

class LocationView extends StatefulWidget {
  LocationView({Key key}) : super(key: key);

  @override
  _LocationViewState createState() => _LocationViewState();
}

class _LocationViewState extends State<LocationView> {
  FlutterBaiduPluginDucafecat _locationPlugin = FlutterBaiduPluginDucafecat();
  StreamSubscription<Map<String, Object>> _locationListener; // 事件监听
  BaiduLocation _baiduLocation; // 经纬度信息
  // Map<String, Object> _loationResult; // 返回格式数据

  @override
  void dispose() {
    super.dispose();

    // 取消监听
    if (null != _locationListener) {
      _locationListener.cancel();
    }
  }

  // 返回定位信息
  void _setupListener() {
    if (_locationListener != null) {
      return;
    }
    _locationListener =
        _locationPlugin.onResultCallback().listen((Map<String, Object> result) {
      setState(() {
        // _loationResult = result;
        try {
          _baiduLocation = BaiduLocation.fromMap(result);
          print(_baiduLocation);
        } catch (e) {
          print(e);
        }
      });
    });
  }

  // 设置android端和ios端定位参数
  void _setLocOption() {
    // android 端设置定位参数
    BaiduLocationAndroidOption androidOption = new BaiduLocationAndroidOption();
    androidOption.setCoorType("bd09ll"); // 设置返回的位置坐标系类型
    androidOption.setIsNeedAltitude(true); // 设置是否需要返回海拔高度信息
    androidOption.setIsNeedAddres(true); // 设置是否需要返回地址信息
    androidOption.setIsNeedLocationPoiList(true); // 设置是否需要返回周边poi信息
    androidOption.setIsNeedNewVersionRgc(true); // 设置是否需要返回最新版本rgc信息
    androidOption.setIsNeedLocationDescribe(true); // 设置是否需要返回位置描述
    androidOption.setOpenGps(true); // 设置是否需要使用gps
    androidOption.setLocationMode(LocationMode.Hight_Accuracy); // 设置定位模式
    androidOption.setScanspan(1000); // 设置发起定位请求时间间隔

    Map androidMap = androidOption.getMap();

    // ios 端设置定位参数
    BaiduLocationIOSOption iosOption = new BaiduLocationIOSOption();
    iosOption.setIsNeedNewVersionRgc(true); // 设置是否需要返回最新版本rgc信息
    iosOption.setBMKLocationCoordinateType(
        "BMKLocationCoordinateTypeBMK09LL"); // 设置返回的位置坐标系类型
    iosOption.setActivityType("CLActivityTypeAutomotiveNavigation"); // 设置应用位置类型
    iosOption.setLocationTimeout(10); // 设置位置获取超时时间
    iosOption.setDesiredAccuracy("kCLLocationAccuracyBest"); // 设置预期精度参数
    iosOption.setReGeocodeTimeout(10); // 设置获取地址信息超时时间
    iosOption.setDistanceFilter(100); // 设置定位最小更新距离
    iosOption.setAllowsBackgroundLocationUpdates(true); // 是否允许后台定位
    iosOption.setPauseLocUpdateAutomatically(true); //  定位是否会被系统自动暂停

    Map iosMap = iosOption.getMap();

    _locationPlugin.prepareLoc(androidMap, iosMap);
  }

  // 启动定位
  void _handleStartLocation() {
    if (null != _locationPlugin) {
      _setupListener();
      _setLocOption();
      _locationPlugin.startLocation();
    }
  }

  // 停止定位
  void _handleStopLocation() {
    if (null != _locationPlugin) {
      _locationPlugin.stopLocation();
      setState(() {
        _baiduLocation = null;
      });
    }
  }

  ////////////////////////////////////////////////////////////

  // 显示地理信息
  Widget _buildLocationView() {
    return _baiduLocation != null
        ? Table(
            children: [
              TableRow(children: [
                TableCell(child: Text('经度')),
                TableCell(child: Text(_baiduLocation.longitude.toString())),
              ]),
              TableRow(children: [
                TableCell(child: Text('纬度')),
                TableCell(child: Text(_baiduLocation.latitude.toString())),
              ]),
              TableRow(children: [
                TableCell(child: Text('国家')),
                TableCell(
                    child: Text(_baiduLocation.country != null
                        ? _baiduLocation.country
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('省份')),
                TableCell(
                    child: Text(_baiduLocation.province != null
                        ? _baiduLocation.province
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('城市')),
                TableCell(
                    child: Text(_baiduLocation.city != null
                        ? _baiduLocation.city
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('区县')),
                TableCell(
                    child: Text(_baiduLocation.district != null
                        ? _baiduLocation.district
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('街道')),
                TableCell(
                    child: Text(_baiduLocation.street != null
                        ? _baiduLocation.street
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('地址')),
                TableCell(
                    child: Text(_baiduLocation.address != null
                        ? _baiduLocation.address
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('位置语义化描述')),
                TableCell(
                    child: Text(_baiduLocation.locationDetail != null
                        ? _baiduLocation.locationDetail
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('周边poi信息')),
                TableCell(
                    child: Text(_baiduLocation.poiList != null
                        ? _baiduLocation.poiList
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('错误码')),
                TableCell(
                    child: Text(_baiduLocation.errorCode != null
                        ? _baiduLocation.errorCode.toString()
                        : "")),
              ]),
              TableRow(children: [
                TableCell(child: Text('定位失败描述信息')),
                TableCell(
                    child: Text(_baiduLocation.errorInfo != null
                        ? _baiduLocation.errorInfo
                        : "")),
              ]),
            ],
          )
        : Container();
  }

  // 控制面板
  Widget _buildControlPlan() {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        MaterialButton(
          color: Colors.blue,
          textColor: Colors.white,
          onPressed: _baiduLocation == null ? _handleStartLocation : null,
          child: Text('开始定位'),
        ),
        MaterialButton(
          color: Colors.blue,
          textColor: Colors.white,
          onPressed: _baiduLocation != null ? _handleStopLocation : null,
          child: Text('暂停定位'),
        )
      ],
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('定位信息'),
      ),
      body: SingleChildScrollView(
        child: Column(
          children: [
            _buildControlPlan(),
            Divider(),
            _buildLocationView(),
          ],
        ),
      ),
    );
  }
}

```

## 参考

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
