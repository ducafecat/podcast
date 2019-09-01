---
title: Flutter 零基础入门中文教学 - 07 了解项目结构及配置文件
date: 2019-08-15 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- Flutter 的项目结构
- Flutter 配置文件讲解
- Flutter 代码结构

## 目录文件结构

![](2019-08-02-16-37-43.png)

| 名称         | 说明             |
| ------------ | ---------------- |
| lib          | Flutter 代码     |
| android      | Android 项目     |
| ios          | IOS 项目         |
| test         | 测试目录         |
| .idea        | IDEA 编辑器配置  |
| pubspec.yaml | Flutter 配置文件 |
| pubspec.lock | 包版本锁定       |
| build        | 编译目录         |

## Pubspec.yaml

```yaml
name: myapp
description: A new Flutter project.

version: 1.0.0+1

environment:
  sdk: ">=2.1.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^0.1.2

  // 我加入了摄像头依赖包
  camera: ^0.5.2+2

dev_dependencies:
  flutter_test:
    sdk: flutter


flutter:

  uses-material-design: true

  # To add assets to your application, add an assets section, like this:
  # assets:
  #  - images/a_dot_burr.jpeg
  #  - images/a_dot_ham.jpeg

  # An image asset can refer to one or more resolution-specific "variants", see
  # https://flutter.dev/assets-and-images/#resolution-aware.

  # For details regarding adding assets from package dependencies, see
  # https://flutter.dev/assets-and-images/#from-packages

  # To add custom fonts to your application, add a fonts section here,
  # in this "flutter" section. Each entry in this list should have a
  # "family" key with the font family name, and a "fonts" key with a
  # list giving the asset and other descriptors for the font. For
  # example:
  # fonts:
  #   - family: Schyler
  #     fonts:
  #       - asset: fonts/Schyler-Regular.ttf
  #       - asset: fonts/Schyler-Italic.ttf
  #         style: italic
  #   - family: Trajan Pro
  #     fonts:
  #       - asset: fonts/TrajanPro.ttf
  #       - asset: fonts/TrajanPro_Bold.ttf
  #         weight: 700
  #
  # For details regarding fonts from package dependencies,
  # see https://flutter.dev/custom-fonts/#from-packages

```

> 保存后 vscode 自动 flutter pub get

## android 目录

### 用 Android Studio 打开

![](image-20190802165057804.png)

### 启动文件

android/app/src/main/java/com/example/myapp/MainActivity.java

![](image-20190802165350585.png)

### gradle 配置文件

android/app/build.gradle

![](image-20190802165633818.png)

### 插件管理文件

android/app/src/main/java/io/flutter/plugins/GeneratedPluginRegistrant.java

```java
package io.flutter.plugins;

import io.flutter.plugin.common.PluginRegistry;
import io.flutter.plugins.camera.CameraPlugin;

/**
 * Generated file. Do not edit.
 */
public final class GeneratedPluginRegistrant {
  public static void registerWith(PluginRegistry registry) {
    if (alreadyRegisteredWith(registry)) {
      return;
    }
    CameraPlugin.registerWith(registry.registrarFor("io.flutter.plugins.camera.CameraPlugin"));
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

## IOS 目录

### 项目

![](image-20190802172320243.png)

### 启动 AppDelegate.m

```objective-c
#include "AppDelegate.h"
#include "GeneratedPluginRegistrant.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [GeneratedPluginRegistrant registerWithRegistry:self];
  // Override point for customization after application launch.
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

@end
```

### Info.plist 配置文件

ios/Runner/Info.plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>CFBundleDevelopmentRegion</key>
	<string>$(DEVELOPMENT_LANGUAGE)</string>
	<key>CFBundleExecutable</key>
	<string>$(EXECUTABLE_NAME)</string>
	<key>CFBundleIdentifier</key>
	<string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
	<key>CFBundleInfoDictionaryVersion</key>
	<string>6.0</string>
	<key>CFBundleName</key>
	<string>myapp</string>
	<key>CFBundlePackageType</key>
	<string>APPL</string>
	<key>CFBundleShortVersionString</key>
	<string>$(FLUTTER_BUILD_NAME)</string>
	<key>CFBundleSignature</key>
	<string>????</string>
	<key>CFBundleVersion</key>
	<string>$(FLUTTER_BUILD_NUMBER)</string>
	<key>LSRequiresIPhoneOS</key>
	<true/>
	<key>UILaunchStoryboardName</key>
	<string>LaunchScreen</string>
	<key>UIMainStoryboardFile</key>
	<string>Main</string>
	<key>UISupportedInterfaceOrientations</key>
	<array>
		<string>UIInterfaceOrientationPortrait</string>
		<string>UIInterfaceOrientationLandscapeLeft</string>
		<string>UIInterfaceOrientationLandscapeRight</string>
	</array>
	<key>UISupportedInterfaceOrientations~ipad</key>
	<array>
		<string>UIInterfaceOrientationPortrait</string>
		<string>UIInterfaceOrientationPortraitUpsideDown</string>
		<string>UIInterfaceOrientationLandscapeLeft</string>
		<string>UIInterfaceOrientationLandscapeRight</string>
	</array>
	<key>UIViewControllerBasedStatusBarAppearance</key>
	<false/>
</dict>
</plist>
```

### ios/Podfile 包管理

```objective-c
# Uncomment this line to define a global platform for your project
# platform :ios, '9.0'

# CocoaPods analytics sends network stats synchronously affecting flutter build latency.
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

project 'Runner', {
  'Debug' => :debug,
  'Profile' => :release,
  'Release' => :release,
}

def parse_KV_file(file, separator='=')
  file_abs_path = File.expand_path(file)
  if !File.exists? file_abs_path
    return [];
  end
  pods_ary = []
  skip_line_start_symbols = ["#", "/"]
  File.foreach(file_abs_path) { |line|
      next if skip_line_start_symbols.any? { |symbol| line =~ /^\s*#{symbol}/ }
      plugin = line.split(pattern=separator)
      if plugin.length == 2
        podname = plugin[0].strip()
        path = plugin[1].strip()
        podpath = File.expand_path("#{path}", file_abs_path)
        pods_ary.push({:name => podname, :path => podpath});
      else
        puts "Invalid plugin specification: #{line}"
      end
  }
  return pods_ary
end

target 'Runner' do
  # Prepare symlinks folder. We use symlinks to avoid having Podfile.lock
  # referring to absolute paths on developers' machines.
  system('rm -rf .symlinks')
  system('mkdir -p .symlinks/plugins')

  # Flutter Pods
  generated_xcode_build_settings = parse_KV_file('./Flutter/Generated.xcconfig')
  if generated_xcode_build_settings.empty?
    puts "Generated.xcconfig must exist. If you're running pod install manually, make sure flutter pub get is executed first."
  end
  generated_xcode_build_settings.map { |p|
    if p[:name] == 'FLUTTER_FRAMEWORK_DIR'
      symlink = File.join('.symlinks', 'flutter')
      File.symlink(File.dirname(p[:path]), symlink)
      pod 'Flutter', :path => File.join(symlink, File.basename(p[:path]))
    end
  }

  # Plugin Pods
  plugin_pods = parse_KV_file('../.flutter-plugins')
  plugin_pods.map { |p|
    symlink = File.join('.symlinks', 'plugins', p[:name])
    File.symlink(p[:path], symlink)
    pod p[:name], :path => File.join(symlink, 'ios')
  }
end

# Prevent Cocoapods from embedding a second Flutter framework and causing an error with the new Xcode build system.
install! 'cocoapods', :disable_input_output_paths => true

post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['ENABLE_BITCODE'] = 'NO'
    end
  end
end
```

### Podfile.lock 锁定包版本

```
PODS:
  - camera (0.0.1):
    - Flutter
  - Flutter (1.0.0)

DEPENDENCIES:
  - camera (from `.symlinks/plugins/camera/ios`)
  - Flutter (from `.symlinks/flutter/ios`)

EXTERNAL SOURCES:
  camera:
    :path: ".symlinks/plugins/camera/ios"
  Flutter:
    :path: ".symlinks/flutter/ios"

SPEC CHECKSUMS:
  camera: d56ad165545ae5a0ffb892376033760a969c68c8
  Flutter: 58dd7d1b27887414a370fcccb9e645c08ffd7a6a

PODFILE CHECKSUM: 7fb83752f59ead6285236625b82473f90b1cb932

COCOAPODS: 1.7.3

```

## Test 目录

![](image-20190802173326105.png)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
