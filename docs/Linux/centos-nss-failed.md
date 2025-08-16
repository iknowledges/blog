# Failed to initialize NSS library

Centos中由于强制卸载包，导致rpm和yum无法使用，出现如下报错：

```
Failed to initialize NSS library
```

强制卸载包的操作如下：

```bash
rpm -e nss-softokn-freebl-3.53.1-6.el7_9.x86_64.rpm --nodeps
```

解决方法如下：

1. 下载对应版本的[nss-softokn-freebl-3.53.1-6.el7_9.x86_64.rpm](https://centos.pkgs.org/7/centos-updates-x86_64/nss-softokn-freebl-3.53.1-6.el7_9.x86_64.rpm.html)，并上传到服务器。

2. 解压rpm包：

```
rpm2cpio nss-softokn-freebl-3.53.1-6.el7_9.x86_64.rpm | cpio -idmv
```

3. 将usr下的文件拷贝到系统环境：

```
yes | cp -R ./usr/lib/* /usr/lib/
yes | cp -R ./usr/lib64/* /usr/lib64/
```
