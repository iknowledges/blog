# WSL中ldconfig报错

### 报错内容

```
/sbin/ldconfig.real: Can't link /usr/lib/wsl/lib/libnvoptix_loader.so.1 to libnvoptix.so.1
/sbin/ldconfig.real: /usr/lib/wsl/lib/libcuda.so.1 is not a symbolic link
```

### 解决方法

1. 重新创建软链接

```
cd /usr/lib/wsl
sudo mkdir lib2
sudo ln -s lib/* lib2
```

2. 修改配置

```
sudo vim /etc/ld.so.conf.d/ld.wsl.conf
```

将`/usr/lib/wsl/lib`改为`/usr/lib/wsl/lib2`

3. 测试是否生效

```
sudo ldconfig
```

#### 参考资料

- [windows下wsl（ubuntu）ldconfig报错](https://blog.csdn.net/qq_47564006/article/details/135056558)
