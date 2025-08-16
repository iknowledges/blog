# Qt搭建安卓开发环境

1. 下载安装[JDK 17](https://www.oracle.com/java/technologies/downloads/)，并设置环境变量：

```bash
export JAVA_HOME=/home/sheeran/Desktop/Software/Java/jdk-17.0.8
export PATH=$PATH:$JAVA_HOME/bin
```

注意：这里必须用JDK 17，官网推荐的JDK 11会报错。

2. 下载安装[Android Studio](https://developer.android.google.cn/studio)

3. 打开【SDK Manager】，勾选【Show Package Details】可查看版本号，对应的版本号可参考[Getting Started with Qt for Android](https://doc.qt.io/qt-6/android-getting-started.html)：

- Android SDK Platform 31
- Android SDK Build-Tools: 31.0.0
- NDK: 25.1.8937393
- Android SDK Command-line Tools: 11.0

![qt-android-1](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-android-1.png)

![qt-android-2](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-android-2.png)

4. 下载android_openssl

```bash
git clone https://github.com/KDAB/android_openssl
```

5. 配置Qt

![qt-android-3](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-android-3.png)
