title: Mac平台 ReactNative iOS&Android 开发环境搭建总结
date: 2015-12-17 22:10:23
categories: code
tags: [ReactNative,Android,iOS]
photos:
- http://img.ptcms.csdn.net/article/201501/29/54c9e8983b9ce_middle.jpg
---

# 环境配置

## 安装Homebrew
Homebrew 是 Mac 中的一个包管理器。没有安装的话，[点击这里安装](http://brew.sh) 我的版本如下:
``` bash
> $ brew -v                                                                                                          ⬡ 5.0.0
Homebrew 0.9.5 (git revision 576d6f; last commit 2015-10-10)
```
版本更新使用：`brew update`

## 安装Node.js 和 npm
Node.js 需要 *4.0*(注意这里，之前就是一直默认使用的是io-js版本导致一直报错) 及其以上版本。安装好之后，npm 也有了
* 通过 `nvm` 安装 `Node.js`
    安装 nvm 可以通过 Homebrew 安装:
    ``` bash
    brew install nvm
    ```
    注：`nvm` 可以管理多个版本的Node和iojs
    然后安装 Node.js：
    ``` bash
    nvm install node && nvm alias default node
    ```
* 安装 `watchman` 和 `flow`
这两个包分别是监控文件变化和类型检查的。安装如下：
``` bash
brew install watchman
brew install flow
```
# 安装 React-Native
通过 npm 安装即可：
``` bash
npm install -g react-native-cli
```
# 软件
* ## iOS
    XCode 6.3 及其以上即可。
* ## Android
    ### Android Studio
    官网下载最新版本安装

    ### SDK下载
    启动 Android Studio， 选择"Preferences"-->"Appearance & Behavior"-->"System Settings"-->"Android SDK"-->"Launch Standalone SDK Manager",勾选以下项目：
    * Android SDK Build-tools version 23.0.1
    * Android 6.0 (API 23)
    * Android Support Repository
    这一步也需要翻墙，经过漫长的等待之后，成功安装。

    ### 定义ANDROID_HOME环境变量
    往你的`~/.bashrc`, `~/.bash_profile` 或者你终端所用的其它配置文件中增加以下内容:
    ```bash
    # 如果你是通过Homebrew安装SDK的，则加入下列路径
    export ANDROID_HOME=/usr/local/opt/android-sdk
    # 否则可能是（当然具体视你把SDK放在哪）：
    export ANDROID_HOME=~/Library/Android/sdk
    ```
    最后执行：
    ``` bash
    source ~/.bash_profile # 立即生效
    ```
<!-- more -->
# 初始化一个项目
在项目根目录执行：
``` bash
react-native init AwesomeProject
```
初始化一个项目，其中 AwesomeProject 是项目名字，可随意修改。等待一段时间之后（具体视网络情况而定，需要翻墙），项目初始化完成。

# 运行项目
* iOS
    `XCode` 打开项目，点击运行就好。修改 `index.ios.js`, 在模拟器中 `⌘ + R` 重新载入 `js` 即可看到相应的变化。
    iOS 真机调试，修改HTTP地址即可。
    ``` bash
    jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle"];
    ```
* Android
    需要一个`Genymotion`模拟器,官方下载个人开发者免费还是挺不错的，[这里是模拟器地址](https://www.genymotion.com/),使用模拟器还需要用到虚拟机，使用免费VirtualBox即可。
    运行命令
    ``` bash
    react-native run-android
    ```
    然后就会部署到模拟器，修改 `index.android.js` ，调出模拟器菜单键，选择重新载入 `js` 即可看到变化。
* Android 真机调试
    示例 APP 直接部署到真机，红色界面报错，无法连接到 Debug Server
    如果是 5.0 或者以上机型，可通过 adb 反向代理端口，将 Mac 端口反向代理到测试机上。
    ``` bash
    adb reverse tcp:8081 tcp:8081
    ```
    如果 5.0 以下机器，应用安装到测试机上之后，摇动设备，在弹出菜单中选择 Dev Setting > Debug Server host for device，然后填入 Mac 的 IP 地址（ifconfig 命令可查看本机 IP）
* 在 Android Studio 中调试开发(暂未尝试)
    可能希望在 Android Studio 打开项目，然后编译部署到真机。
    这个时候，在命令行启动 Debug Server 即可：
    ``` bash
    react-native start
    ```
