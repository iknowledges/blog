# nodejs常用命令

```
# 设置淘宝镜像
npm config set registry https://npmmirror.com/
# 查看指定配置
npm config get registry
# 查看全部配置
npm config ls -l
# 清除install的缓存
npm cache clear --force
# 查看已安装的包
npm list -g --depth=0
# 指定镜像安装cnpm
npm install -g cnpm --registry=https://registry.npmmirror.com
# 卸载包
npm uninstall -g cnpm
```

#### 参考资料

- [淘宝镜像站](https://npmmirror.com/)
