---
title: Flutter 混合开发 - 03 百度地图定位功能 ios 篇
date: 2020-08-21 00:00:00
tags: flutter
categories: Flutter混合开发
---

![](2020-08-21-07-19-43.png)

# 本节目标

![](2020-08-24-10-01-58.png)

- 创建 ios flutter 插件流程
- 集成百度定位功能

## 视频

https://www.bilibili.com/video/BV1HT4y1L73i/

<iframe src="//player.bilibili.com/player.html?bvid=BV1HT4y1L73i&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

## 代码

https://github.com/ducafecat/flutter_baidu_plugin_ducafecat/releases/tag/v1.0.3

## 百度平台部分

### 设置 AK

https://lbsyun.baidu.com/apiconsole/key#/home

- 添加应用

![](2020-08-21-05-56-56.png)

- 查看 Bundle Identifier

![](2020-08-21-05-59-34.png)

## IOS 部分

### 自动部署 CocoaPods

- 安装工具

```
sudo gem install cocoapods
```

- ios/flutter_baidu_plugin_ducafecat.podspec

```
  ...
  s.dependency 'Flutter'
  s.platform = :ios, '8.0'

  s.dependency 'BMKLocationKit'

  # Flutter.framework does not contain a i386 slice. Only x86_64 simulators are supported.
  s.pod_target_xcconfig = { 'DEFINES_MODULE' => 'YES', 'VALID_ARCHS[sdk=iphonesimulator*]' => 'x86_64' }
end
```

- 安装百度 sdk 包

```
pod install
```

- 升级

```
pod repo update
```

- pod search 无法搜索到类库的解决办法（找不到类库）

```
（1）执行pod setup

（2）删除~/Library/Caches/CocoaPods目录下的search_index.json文件

    pod setup成功后会生成~/Library/Caches/CocoaPods/search_index.json文件

    终端输入rm ~/Library/Caches/CocoaPods/search_index.json

    删除成功后再执行pod search

（3）执行pod search
```

### Info.plist 定位授权

example/ios/Runner/Info.plist

```
<dict>
	<key>NSLocationWhenInUseUsageDescription</key>
	<string>需要定位</string>
```

### 消息通知 BdmapFlutterStreamManager

- ios/Classes/BdmapFlutterStreamManager.h

```cpp
//
//  Header.h
//  bdmap_location_flutter_plugin
//
//  Created by Wang,Shengzhan on 2020/2/4.
//


#import <Foundation/Foundation.h>
#import <Flutter/Flutter.h>

NS_ASSUME_NONNULL_BEGIN
@class BdmapFlutterStreamHandler;
@interface BdmapFlutterStreamManager : NSObject
+ (instancetype)sharedInstance ;
@property (nonatomic, strong) BdmapFlutterStreamHandler* streamHandler;

@end

@interface BdmapFlutterStreamHandler : NSObject<FlutterStreamHandler>
@property (nonatomic, strong) FlutterEventSink eventSink;

@end
NS_ASSUME_NONNULL_END
```

- ios/Classes/BdmapFlutterStreamManager.m

```cpp
//
//  BdmapFlutterStreamManager.m
//  bdmap_location_flutter_plugin
//
//  Created by Wang,Shengzhan on 2020/2/4.
//

#import "BdmapFlutterStreamManager.h"

@implementation BdmapFlutterStreamManager

+ (instancetype)sharedInstance {
    static dispatch_once_t onceToken;
    static BdmapFlutterStreamManager *manager = nil;
    dispatch_once(&onceToken, ^{
        manager = [[BdmapFlutterStreamManager alloc] init];
        BdmapFlutterStreamHandler * streamHandler = [[BdmapFlutterStreamHandler alloc] init];
        manager.streamHandler = streamHandler;
    });

    return manager;
}

@end


@implementation BdmapFlutterStreamHandler

- (FlutterError*)onListenWithArguments:(id)arguments eventSink:(FlutterEventSink)eventSink {
    self.eventSink = eventSink;
    return nil;
}

- (FlutterError*)onCancelWithArguments:(id)arguments {

    return nil;
}

@end
```

### 地图接口业务 FlutterBaiduPluginDucafecatPlugin

- ios/Classes/FlutterBaiduPluginDucafecatPlugin.h

```cpp
#import <Flutter/Flutter.h>

@interface FlutterBaiduPluginDucafecatPlugin : NSObject<FlutterPlugin>
@end
```

- ios/Classes/FlutterBaiduPluginDucafecatPlugin.m

```cpp
#import "FlutterBaiduPluginDucafecatPlugin.h"
#import "BMKLocationkit/BMKLocationComponent.h"
#import "BdmapFlutterStreamManager.h"

@interface FlutterBaiduPluginDucafecatPlugin()<BMKLocationManagerDelegate>

@property (nonatomic,strong) BMKLocationManager *locManager;

@property (nonatomic, copy) FlutterResult flutterResult;

@end



@implementation FlutterBaiduPluginDucafecatPlugin
+ (void)registerWithRegistrar:(NSObject<FlutterPluginRegistrar>*)registrar {
  // FlutterMethodChannel* channel = [FlutterMethodChannel
  //     methodChannelWithName:@"flutter_baidu_plugin_ducafecat"
  //           binaryMessenger:[registrar messenger]];
  // FlutterBaiduPluginDucafecatPlugin* instance = [[FlutterBaiduPluginDucafecatPlugin alloc] init];
  // [registrar addMethodCallDelegate:instance channel:channel];

    FlutterMethodChannel* channel = [FlutterMethodChannel
                                     methodChannelWithName:@"flutter_baidu_plugin_ducafecat"
                                     binaryMessenger:[registrar messenger]];

    FlutterBaiduPluginDucafecatPlugin* instance = [[FlutterBaiduPluginDucafecatPlugin alloc] init];

    [registrar addMethodCallDelegate:instance channel:channel];

    FlutterEventChannel *eventChanel = [FlutterEventChannel eventChannelWithName:@"flutter_baidu_plugin_ducafecat_stream" binaryMessenger:[registrar messenger]];

    [eventChanel setStreamHandler:[[BdmapFlutterStreamManager sharedInstance] streamHandler]];

}

// - (void)handleMethodCall:(FlutterMethodCall*)call result:(FlutterResult)result {
//   if ([@"getPlatformVersion" isEqualToString:call.method]) {
//     result([@"iOS " stringByAppendingString:[[UIDevice currentDevice] systemVersion]]);
//   }
//   else if ([@"duAddOne" isEqualToString:call.method]) {
//       NSInteger val = 100;
//       val += [[call.arguments objectForKey:@"num"] intValue];
//       result([NSNumber numberWithLong:val]);
//   }
//   else {
//     result(FlutterMethodNotImplemented);
//   }
// }


- (void)handleMethodCall:(FlutterMethodCall*)call result:(FlutterResult)result {

    if ([@"getPlatformVersion" isEqualToString:call.method]) {

        result([@"iOS " stringByAppendingString:[[UIDevice currentDevice] systemVersion]]);

    } else if ([@"startLocation" isEqualToString:call.method]){ // 开始定位

//        NSLog((@"\n[bdmap_loc_flutter_plugin:%s]"), "startLocation...");
        [self startLocation:result];

    }else if ([@"stopLocation" isEqualToString:call.method]){ // 停止定位

//        NSLog((@"\n[bdmap_loc_flutter_plugin:%s]"), "stopLocation...");
        [self stopLocation];
        result(@YES);

    } else if ([@"updateOption" isEqualToString:call.method] ) { // 设置定位参数

        result(@([self updateOption:call.arguments]));

    } else if ([@"setApiKey" isEqualToString:call.method]){ // 设置ios端ak
//        NSLog((@"\n[bdmap_loc_flutter_plugin:%s]"), "setApiKey...");
        [[BMKLocationAuth sharedInstance] checkPermisionWithKey:call.arguments authDelegate:self];
        result(@YES);
    } else {

        result(FlutterMethodNotImplemented);
    }
}

/**
 获取设置的期望定位精度
 */
-(double)getDesiredAccuracy:(NSString*)str{

    if([@"kCLLocationAccuracyBest" isEqualToString:str]) {
        return kCLLocationAccuracyBest;
    } else if ([@"kCLLocationAccuracyNearestTenMeters" isEqualToString:str]) {
        return kCLLocationAccuracyNearestTenMeters;
    } else if ([@"kCLLocationAccuracyHundredMeters" isEqualToString:str]) {
        return kCLLocationAccuracyHundredMeters;
    } else if ([@"kCLLocationAccuracyKilometer" isEqualToString:str]) {
        return kCLLocationAccuracyKilometer;
    } else {
        return kCLLocationAccuracyBest;
    }
}

/**
 获取设置的经纬度坐标系类型
 */
-(int)getCoordinateType:(NSString*)str{

    if([@"BMKLocationCoordinateTypeBMK09LL" isEqualToString:str]) {
        return BMKLocationCoordinateTypeBMK09LL;
    } else if ([@"BMKLocationCoordinateTypeBMK09MC" isEqualToString:str]) {
        return BMKLocationCoordinateTypeBMK09MC;
    } else if ([@"BMKLocationCoordinateTypeWGS84" isEqualToString:str]) {
        return BMKLocationCoordinateTypeWGS84;
    } else if ([@"BMKLocationCoordinateTypeGCJ02" isEqualToString:str]) {
        return BMKLocationCoordinateTypeGCJ02;
    } else {
        return BMKLocationCoordinateTypeGCJ02;
    }


}


/**
 获取设置的应用位置类型
 */
-(int)getActivityType:(NSString*)str{

    if ([@"CLActivityTypeOther" isEqualToString:str]) {
        return CLActivityTypeOther;
    } else if ([@"CLActivityTypeAutomotiveNavigation" isEqualToString:str]) {
        return CLActivityTypeAutomotiveNavigation;
    } else if ([@"CLActivityTypeFitness" isEqualToString:str]) {
        return CLActivityTypeFitness;
    } else if ([@"CLActivityTypeOtherNavigation" isEqualToString:str]) {
        return CLActivityTypeOtherNavigation;
    } else {
        return CLActivityTypeAutomotiveNavigation;
    }


}

/**
 解析flutter端所设置的定位SDK参数
 */
-(BOOL)updateOption:(NSDictionary*)args {
    if(self.locManager) {

//        NSLog(@"定位参数配置:%@",args);

        self.locManager.isNeedNewVersionReGeocode = YES;

        // 设置期望定位精度
        if ([[args allKeys] containsObject:@"desiredAccuracy"]) {
          [self.locManager setDesiredAccuracy:[ self getDesiredAccuracy: args[@"desiredAccuracy"]]];
        }

        // 设置定位的最小更新距离
        if ([[args allKeys] containsObject:@"distanceFilter"]) {
            self.locManager.distanceFilter = [args[@"distanceFilter"] doubleValue];
//            NSLog(@"最小更新距离值:%f", [args[@"distanceFilter"] doubleValue]);
        }

        // 设置返回位置坐标系类型
        if ([[args allKeys] containsObject:@"BMKLocationCoordinateType"]) {
            [self.locManager setCoordinateType:[ self getCoordinateType: args[@"desiredAccuracy"]]];
        }

        // 设置应用位置类型
        if ([[args allKeys] containsObject:@"activityType"]) {
            [self.locManager setActivityType:[ self getActivityType: args[@"desiredAccuracy"]]];
        }

        // 设置是否需要返回新版本rgc信息
        if ([[args allKeys] containsObject:@"isNeedNewVersionRgc"]) {
            if ((bool)args[@"desiredAccuracy"]) {
//                NSLog(@"需要返回新版本rgc信息");
                self.locManager.isNeedNewVersionReGeocode = YES;
            } else {
//                NSLog(@"不需要返回新版本rgc信息");
                self.locManager.isNeedNewVersionReGeocode = NO;
            }

        }

        // 指定定位是否会被系统自动暂停
        if ([[args allKeys] containsObject:@"pausesLocationUpdatesAutomatically"]) {
            if ((bool)args[@"pausesLocationUpdatesAutomatically"]) {
//                NSLog(@"设置定位被系统自动暂停");
                self.locManager.isNeedNewVersionReGeocode = YES;
            } else {
//                NSLog(@"设置定位不能被系统自动暂停");
                self.locManager.isNeedNewVersionReGeocode = NO;
            }

        }

        // 设置是否允许后台定位
        if ([[args allKeys] containsObject:@"allowsBackgroundLocationUpdates"]) {
             if ((bool)args[@"allowsBackgroundLocationUpdates"]) {
//                  NSLog(@"设置允许后台定位");
                  self.locManager.isNeedNewVersionReGeocode = YES;
             } else {
//                  NSLog(@"设置不允许后台定位");
                  self.locManager.isNeedNewVersionReGeocode = NO;
                 self.locManager.distanceFilter = kCLDistanceFilterNone;
             }

        }

        // 设置定位超时时间
        if ([[args allKeys] containsObject:@"locationTimeout"]) {
            [self.locManager setLocationTimeout:[args[@"locationTimeout"] integerValue]];
            self.locManager.coordinateType = BMKLocationCoordinateTypeGCJ02;
        }

        // 设置逆地理超时时间
        if ([[args allKeys] containsObject:@"reGeocodeTimeout"]) {
            [self.locManager setReGeocodeTimeout:[args[@"reGeocodeTimeout"] integerValue]];
        }

        return YES;

    }
    return NO;
}
/**
 启动定位
 */
- (void)startLocation:(FlutterResult)result
{
    self.flutterResult = result;
    [self.locManager startUpdatingLocation];
}

/**
 停止定位
 */
- (void)stopLocation
{
    self.flutterResult = nil;
    [self.locManager stopUpdatingLocation];
}

- (BMKLocationManager *)locManager {
    if (!_locManager) {
        _locManager = [[BMKLocationManager alloc] init];
        _locManager.locatingWithReGeocode = YES;
        _locManager.delegate = self;
    }
    return _locManager;
}

/**
 *  @brief 连续定位回调函数
 *  @param manager 定位 BMKLocationManager 类。
 *  @param location 定位结果。
 */
- (void)BMKLocationManager:(BMKLocationManager * _Nonnull)manager didUpdateLocation:(BMKLocation * _Nullable)location orError:(NSError * _Nullable)error

{
    if (error)
    {
//        NSLog(@"locError:{%ld - %@};", (long)error.code, error.localizedDescription);
    } if (location) { // 得到定位信息，添加annotation
        NSMutableDictionary *dic = [NSMutableDictionary dictionaryWithCapacity:1];

        if (location) {

            if (location.location.timestamp) {
                [dic setObject:[self getFormatTime:location.location.timestamp] forKey:@"locTime"]; // 定位时间
            }


            if (location.location.horizontalAccuracy) {
                [dic setObject:@(location.location.horizontalAccuracy) forKey:@"radius"]; // 定位精度
            }


            if (location.location.coordinate.latitude) {
                [dic setObject:@(location.location.coordinate.latitude) forKey:@"latitude"]; // 纬度
            }

            if (location.location.coordinate.longitude) {
                [dic setObject:@(location.location.coordinate.longitude) forKey:@"longitude"]; // 经度
            }

            if (location.location.altitude) {
//                NSLog(@"返回海拔高度信息");
                [dic setObject:@(location.location.altitude) forKey:@"altitude"];// 高度
            }


            if (location.rgcData) {
                [dic setObject:[location.rgcData country] forKey:@"country"]; // 国家
                [dic setObject:[location.rgcData province] forKey:@"province"]; // 省份
                [dic setObject:[location.rgcData city] forKey:@"city"]; // 城市
                if (location.rgcData.district) {
                    [dic setObject:[location.rgcData district] forKey:@"district"]; // 区县
                }

                if (location.rgcData.street) {
                    [dic setObject:[location.rgcData street] forKey:@"street"]; // 街道
                }

                if (location.rgcData.description) {
                    // 地址信息
                    [dic setObject:[location.rgcData description] forKey:@"address"];
                }


                if (location.rgcData.poiList) {

                    NSString* poilist;

                    if (location.rgcData.poiList.count == 1) {

                        for (BMKLocationPoi * poi in location.rgcData.poiList) {
                            poilist = [[poi name] stringByAppendingFormat:@",%@,%@", [poi tags], [poi addr]];
                        }

                    } else {
                        for (int i = 0; i < location.rgcData.poiList.count - 1 ; i++) {
                            poilist = [poilist stringByAppendingFormat:@"%@,%@,%@|", location.rgcData.poiList[i].name,location.rgcData.poiList[i].tags,location.rgcData.poiList[i].addr];
                        }

                        poilist = [poilist stringByAppendingFormat:@"%@,%@,%@",
                                location.rgcData.poiList[location.rgcData.poiList.count-1].name,location.rgcData.poiList[location.rgcData.poiList.count-1].tags,location.rgcData.poiList[location.rgcData.poiList.count-1].addr];

                    }
                    [dic setObject: poilist forKey:@"poiList"]; // 周边poi信息

                }
            }
        } else {
            [dic setObject: @1 forKey:@"errorCode"]; // 定位结果错误码
            [dic setObject:@"location is null" forKey:@"errorInfo"]; // 定位错误信息
        }

        // 定位结果回调时间
        [dic setObject:[self getFormatTime:[NSDate date]] forKey:@"callbackTime"];

        [[BdmapFlutterStreamManager sharedInstance] streamHandler].eventSink(dic);
//        NSLog(@"x=%f,y=%f",location.location.coordinate.latitude,location.location.coordinate.longitude);
    }
}

/**
 格式化时间
 */
- (NSString *)getFormatTime:(NSDate*)date
{
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    [formatter setDateFormat:@"YYYY-MM-dd HH:mm:ss"];
    NSString *timeString = [formatter stringFromDate:date];
    return timeString;
}

@end
```

## Flutter 部分

### 设置 AK

```dart
  @override
  void initState() {
    super.initState();
    _requestPermission(); // 执行权限请求

    if (Platform.isIOS == true) {
      FlutterBaiduPluginDucafecat.setApiKeyForIOS(
          "dkYT07blcAj3drBbcN1eGFYqt16HP1pR");
    }
  }
```

> 其它代码和 android 同接口 无影响

## 参考

- https://lbsyun.baidu.com/apiconsole/key#/home
- https://flutter.dev/docs/development/packages-and-plugins/developing-packages#step-2c-add-ios-platform-code-swifthm
- http://lbsyun.baidu.com/index.php?title=ios-locsdk/guide/create-project/cocoapods-create
- http://lbsyun.baidu.com/index.php?title=ios-locsdk

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
