# React Native开发环境搭建

## Android开发环境

1. 下载和安装Android Studio：https://developer.android.google.cn/studio/

2. 配置环境变量

修改`~/.bashrc`

```bash
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/emulator
```

使用`source ~/.bashrc`使环境变量生效。

## 编译并运行React Native应用

将`android/build.gradle`中的`mavenCentral()`和`google()`分别替换为`maven { url 'https://maven.aliyun.com/repository/central' }`和`maven { url 'https://maven.aliyun.com/repository/google' }`

```bash
# 使用最新版创建项目
npx react-native@latest init AwesomeProject
# 使用指定版本创建项目
npx react-native@0.72.4 init AwesomeProject --version 0.72.4
cd AwesomeProject
# 运行android环境
npx react-native run-android
# 运行项目
npx react-native start
```

如果找不到模拟器则执行下面命令：

```bash
emulator -list-avds
emulator -avd <name>
```