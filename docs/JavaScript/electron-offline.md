# Electron离线打包教程

## 安装Electron

```
npm install --save-dev electron
```

## 安装Electron Forge

```
npm install --save-dev @electron-forge/cli
npx electron-forge import
```

## 离线安装

在有网的条件下安装好Electron后，将要打包的项目文件以及缓存拷贝到离线环境，比如我的安装好了electron 30.0.8，缓存在%LOCALAPPDATA%/electron/Cache/8835f4c1c633a224dbe42ed5e41c45971a828de023a26b63030f454d77d21c2f/electron-v30.0.8-win32-x64.zip，那就把目录8835f4c1c633a224dbe42ed5e41c45971a828de023a26b63030f454d77d21c2f/都拷贝过去。

各操作系统对应的electron缓存路径分别为：

- Linux: $XDG_CACHE_HOME or ~/.cache/electron/
- MacOS: ~/Library/Caches/electron/
- Windows: %LOCALAPPDATA%/electron/Cache or ~/AppData/Local/electron/Cache/

#### 参考资料

- [不联网的情况下，使用 electron-builder 快速打包全平台应用](https://segmentfault.com/a/1190000041491740)