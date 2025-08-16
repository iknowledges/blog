# Flutter安装教程

## 安装Flutter

1. Linux需要安装的依赖

```bash
sudo apt-get install clang cmake ninja-build pkg-config libgtk-3-dev liblzma-dev
```

2. 解压安装包

```bash
tar xf flutter_linux_3.10.6-stable.tar.xz
```

3. 配置环境变量

```bash
# Flutter
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PATH=$PATH:/home/sheeran/Desktop/Software/flutter/bin
```

4. 验证是否安装成功

```bash
flutter --version
dart --version
flutter docker
```

5. 同意Android协议

```bash
flutter doctor --android-licenses
```

## 安装Android Studio

1. 解压安装包

```bash
tar -zxvf android-studio-2022.3.1.20-linux.tar.gz
```

2. 运行studio.sh按提示安装SDK，然后配置环境变量

```bash
# Android Studio
export ANDROID_HOME=/home/sheeran/Desktop/Software/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools
```

## 问题解决

- Android下载速度过慢，修改build.gradle：

```
repositories {
    // google()
    // mavenCentral()
    maven { url 'https://maven.aliyun.com/repository/central' }
    maven { url 'https://maven.aliyun.com/repository/google' }
}
```

- Ubuntu22.04运行桌面版报错：

```
/usr/bin/ld: 找不到 -lstdc++: 没有那个文件或目录
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

安装如下依赖：

```bash
sudo apt install libstdc++-12-dev
```

- flutter run failed with permission denied (copy to /usr/local/)

```
flutter clean
flutter run -d linux
```

#### 参考资料

- [在中国网络环境下使用Flutter](https://flutter.cn/community/china)
- [Android环境变量](https://developer.android.google.cn/studio/command-line/variables?hl=zh-cn)