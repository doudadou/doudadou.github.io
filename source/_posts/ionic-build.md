title: 使用ionic构建iOS与Android客户端
date: 2015-10-30 14:46:56
categories: code
tags: [ionic,iOS,Android,cordova]
photos:
- http://ionicframework.com/img/angular-ionic.png
---
# iOS
``` bash
sudo npm install -g cordova ionic ios-sim
```

ionic官网为开发者提供了多个开发模板，如：

空白模板（Black app）：
``` bash
ionic start myApp blank
```

tabs模板 ：
``` bash
ionic start myApp tabs
```


sidemenu模板：
``` bash   
ionic start myApp sidemenu
```
## Run
``` bash
cd myIonicApp
ionic platform add ios
ionic build ios
ionic prepare ios  重新打包
ionic emulate ios
ionic emulate ios -livereload
ionic platform remove ios
```
<!-- more -->
# Android
## Run
``` bash
cd myIonicApp
ionic platform add android
ionic build android
ionic emulate android (模拟器运行)
ionic run android (连接上手机运行)
ionic prepare android 重新打包
ionic platform remove android 移除环境
```
### 下载Android Studio
http://developer.android.com/sdk/index.html

选择Tools > Android > `SDK Manager`
选择下方`Launch Standalone SDK Manager`更新sdk

### 设置环境变量
```bash
export ANDROID_HOME="/Users/Kai/Library/Android/sdk"
export ANDROID_PLATFORM_TOOLS="/Users/Kai/Library/Android/sdk/platform-tools"
export PATH="$PATH:$ANDROID_HOME/tools:$ANDROID_PLATFORM_TOOLS"
```
>来源：http://stackoverflow.com/questions/28076575/phonegap-cordova-no-such-file-build-template

选择Tools > Android > `Sync Project with Gradle Files`

### 安装java
android 5.0开始默认安装jdk1.7才能编译，但是由于mac系统自带jdk的版本是1.6，所以需要手动下载jdk1.7并配置

下载新Java
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

Mac OSX 10.9以后系统就自带了Java 6的环境，路径在:
```bash
/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
```
如果想要安装升级到Java 7的环境，步骤如下：

1.到Oracle官网下载系统对应JDK7的安装包, 地址在这里,安装成功后JDK7默认的路径在:
``````bash
/Library/Java/JavaVirtualMachines/jdk1.7.0_60.jdk/Contents/Home
```
2.安装成功后配置环境变量
在.bash_profile文件中添加：
```bash
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
```
需要说明的是Mac OSX 10.5之后苹果就建议设置`$JAVA_HOME`变量到`/usr/libexec/java_home`

3.设置完成后输入下列命令测试下
```bash
$java -version
java version "1.7.0_60"
Java(TM) SE Runtime Environment (build 1.7.0_60-b19)
Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode)

# 查看系统安装的java版本
$/usr/libexec/java_home -V
Matching Java Virtual Machines (3):
1.7.0_60, x86_64:	"Java SE 7"	/Library/Java/JavaVirtualMachines/jdk1.7.0_60.jdk/Contents/Home
1.6.0_65-b14-462, x86_64:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
1.6.0_65-b14-462, i386:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home

# 返回系统安装的java最高版本
$/usr/libexec/java_home
/Library/Java/JavaVirtualMachines/jdk1.7.0_60.jdk/Contents/Home
```
>来源：http://stormzhang.com/android/2014/06/27/manage-java-on-macosx/

#### 问题
>Exception in thread "main" java.lang.RuntimeException: java.util.zip.ZipException: error in opening zip file
at org.gradle.wrapper.

- 下载Gradle( http://services.gradle.org/distributions/gradle-2.2.1-all.zip)
复制它放到这里变成这样
`myApp\platforms\android\gradle\gradle-2.2.1-all.zip`
然后编辑`myApp\platforms\android\cordova\lib\build.js`

```javascript
 var distributionUrl = 'distributionUrl=http\\://services.gradle.org/distributions/gradle-2.2.1-all.zip';
 ```
 替换成
```javascript
var distributionUrl = 'distributionUrl=../gradle-2.2.1-all.zip';
```
>来源：http://stackoverflow.com/questions/29874564/ionic-build-android-error-when-download-gradle


