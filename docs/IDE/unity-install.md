# Unity安装教程

1. 下载[Unity Hub](https://docs.unity.com/en-us/hub/install-hub)，然后双击安装。
2. 启动Unity Hub，点击【Settings】，自定义好【Projects】中的【Project location】，【Installs】中的【Installs location】和【Downloads location】，然后再选择合适的Editor进行安装。
3. 如果碰到网络问题请将代理设置为TUN模式，先删除位于`C:\Users\<你的用户名>\AppData\Roaming\UnityHub\Cache`的缓存，然后再重新下载。
4. 使用【Windows】->【Package Manager】->【Install package from git URL】安装依赖包失败时，可以用下面bat脚本启动Unity后再尝试安装：

```
@echo off
set HTTP_PROXY=http://127.0.0.1:1080
set HTTPS_PROXY=http://127.0.0.1:1080
set NODE_TLS_REJECT_UNAUTHORIZED=0
start "" "D:\Program Files\Unity Hub\Unity Hub.exe"
```

#### 参考资料

- [UnityHub Validation Failed下载编辑器错误，添加模块报错的解决方案](https://u9king.github.io/2025/08/10/UnityHub%20ValidationFailed%E4%B8%8B%E8%BD%BD%E7%BC%96%E8%BE%91%E5%99%A8%E9%94%99%E8%AF%AF%EF%BC%8C%E6%B7%BB%E5%8A%A0%E6%A8%A1%E5%9D%97%E6%8A%A5%E9%94%99%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88/)
- [unity设置代理，以及解决下载依赖包报错 unable to verify the first certificate的问题](https://blog.csdn.net/grace_yi/article/details/108378873)