22223333私人记录。有些事真是做一次抓狂一次，考虑到RN的不稳定性，也许哪次崩了就得迁移，为了到时不会手忙脚乱，特此记录！

# [react-native-blur](https://github.com/react-native-community/react-native-blur)

### Android

1. 在 android/app/build.gradle 中配置：

```
android {
  buildToolsVersion '23.0.3'

  defaultConfig {
    renderscriptTargetApi 23
    renderscriptSupportModeEnabled true
  }
}
```



# [react-native-image-crop-picker](https://github.com/ivpusic/react-native-image-crop-picker)

### iOS

1. 在 Info.plist 中配置：
```
<key>NSPhotoLibraryUsageDescription</key>
<string>特特罗想访问你的相册喔 ~</string>
```

2. 将 node_modules/react-native-image-crop-picker/ios/ 中的 ImageCropPickerSDK 目录拖拽进项目，勾选 Copy items if needed

3. 项目配置 -> General -> Deployment Info，Deployment Target 设为 8.0

4. 项目配置 -> General -> Embedded Binaries，添加 RSKImageCropper.framework 与
 QBImagePicker.framework

**imageCropPicker有BUG，必须采用以下非官方的额外配置**
[详见这个issue中chuece的解决方案](https://github.com/ivpusic/react-native-image-crop-picker/issues/414)

5. 将 node_modules/react-native-image-crop-picker/ios/ 中的 imageCropPicker.xcodeproj 拖拽进项目的 Libraries 目录（若已存在，则略过这步）

6. 项目配置 -> General -> Linked frameworks and libraries，添加一项 libimageCropPicker.a

7. 找到 Libraries/imageCropPicker.xcodeproj/ImageCropPicker.h
将其中的：
``#import "QBImagePickerController/QBImagePickerController.h"``
改为：
``#import "QBImagePicker/QBImagePickerController.h"``

### Android

1. 在 android/app/build.gradle 中配置：

```
android {
  defaultConfig {
    vectorDrawables.useSupportLibrary = true
  }
}
```



# [react-native-push-notification](https://github.com/zo0r/react-native-push-notification)

### iOS

1. 将 node_modules/react-native/Libraries/PushNotificationIOS/ 中的 RCTPushNotification.xcodeproj 拖拽进项目的 Libraries 目录

2. 项目配置 -> Build Phases -> Link Binary With Libraries，添加一项 libRCTPushNotification.a

3. 在 AppDelegate.m 中添加代码：

```
#import <React/RCTPushNotificationManager.h>
```

以及：

```
// Required to register for notifications
- (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
{
  [RCTPushNotificationManager didRegisterUserNotificationSettings:notificationSettings];
}
// Required for the register event.
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
{
  [RCTPushNotificationManager didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
}
// Required for the notification event. You must call the completion handler after handling the remote notification.
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
{
  [RCTPushNotificationManager didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
}
// Required for the registrationError event.
- (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
{
  [RCTPushNotificationManager didFailToRegisterForRemoteNotificationsWithError:error];
}
// Required for the localNotification event.
- (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
{
  [RCTPushNotificationManager didReceiveLocalNotification:notification];
}
```

### Android

1. 在 android/app/build.gradle 中配置：

```
dependencies {
  compile project(':react-native-push-notification')
  compile ('com.google.android.gms:play-services-gcm:8.1.0') {
    force = true;
  }
}
```

2. 在 AndroidManifest.xml 中配置：

```
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<permission
  android:name="${applicationId}.permission.C2D_MESSAGE"
  android:protectionLevel="signature"
/>
<uses-permission android:name="${applicationId}.permission.C2D_MESSAGE"/>
<uses-permission android:name="android.permission.VIBRATE"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

以及

```
<receiver
  android:name="com.google.android.gms.gcm.GcmReceiver"
  android:exported="true"
  android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
    <category android:name="${applicationId}"/>
  </intent-filter>
</receiver>

<receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher"/>

<receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED"/>
  </intent-filter>
</receiver>

<service android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationRegistrationService"/>

<service
  android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
  android:exported="false">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
  </intent-filter>
</service>
```



# [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)

### iOS

1. 将 node_modules/react-native-vector-icons/Fonts/ 中需要的字体拖拽进 Resources 目录

2. 在 Info.plist 的 Fonts provided by application 一项下配置字体，比如：

![](https://cloud.githubusercontent.com/assets/378279/12421498/2db1f93a-be88-11e5-89c8-2e563ba6251a.png)

### Android

1. 将 node_modules/react-native-vector-icons/Fonts/ 中需要的字体复制到
 android/app/src/main/assets/fonts/ 目录



# [react-native-webp](https://github.com/dbasedow/react-native-webp)

### iOS

1. 右键点击项目，通过 Add Files to ... 添加 node_modules/react-native-webp/ 中的 WebP.framework 与 WebPDemux.framework

2. 项目配置 -> Build Settings -> Framework Search Path，添加一项：
``$(SRCROOT)/../node_modules/react-native-webp``