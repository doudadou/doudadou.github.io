title: ionic技巧与坑
date: 2015-09-27 21:51:23
categories: code
tags: [ionic,Angularjs]
photos:
- http://ionicframework.com/img/native.png
---
## 应用Build时报错 Cordova app failing to Archive with Xcode 7.1 (Cordova/CDVViewController.h file not found)

### 解决方法
加入这行命令在 `Build Settings -> Header Search Paths`:
``` bash
"$(OBJROOT)/UninstalledProducts/$(PLATFORM_NAME)/include"
```
## Ionic IOS9访问http受到限制
### 错误：

>Application Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.    

### 解决方法：

手动在 `项目名-Info.plist`第一个`<dict>`标签下添加下面标签
```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
</dict>
```
## cordova 插件使用命令
```bash
//安装插件
cordova plugin add org.apache.cordova.device

//查看插件列表
cordova plugin list

//删除插件
cordova plugin remove org.apache.cordova.device
``` bash
## 隐藏topbar
```html
<ion-view hide-nav-bar="true">
```
## 关闭安卓转场特效
```javascript
$ionicConfigProvider. platform.android.views.transition=none
maxCache也要设置成0
```
## 多modal显示问题
```javascript
$ionicModal.fromTemplateUrl('templates/modal-login.html', {
        id: 'login',
        scope: $scope,
        animation: 'slide-in-up'
    }).then(function(modal) {
        $scope.modalLogin = modal;
    });

    $scope.openModal = function(index) {
      if (index === 'login'){
        $scope.modalLogin.show();
      }
    };
    $scope.closeModal = function() {
        $scope.modalLogin.hide();
    };
    $scope.$on('$destroy', function() {
      $scope.modalLogin.remove();
    });
```
>加入id 索引值来区分多个modal
<!-- more -->
## 使Chrome和Chromium实现跨域请求
通过命令行启动:

```bash
open -a "Google Chrome" --args --disable-web-security
open -a "Chromium" --args --disable-web-security
```

## cordova/ionic emulate 时选择模拟器版本（iOS）
在项目的根目录下：
```bash
$ ./platforms/ios/cordova/lib/list-emulator-images
iPhone-4s
iPhone-5
iPhone-5s
iPhone-6-Plus
iPhone-6
iPad-2
iPad-Retina
iPad-Air
Resizable-iPhone
Resizable-iPad
```
运行模拟器任务
```bash
ionic run ios --target iPhone-5
```
>注意：iPhone别写成iphone。
