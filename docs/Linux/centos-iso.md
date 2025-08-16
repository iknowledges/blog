# CentOS7.6挂载ISO离线安装依赖

1. vmware安装CentOS，确保CD/DVD的设备状态为已连接。

2. 挂载设备

命令格式：mount -t [设备类型] [被挂载的设备名] [挂载处目录名]

```bash
# 创建挂载目录
mkdir -p /mnt/cdrom
# 挂载命令
mount -t auto /dev/cdrom /mnt/cdrom
# 查看是否挂载成功
ls /mnt/cdrom
# 取消挂载
umount /mnt/cdrom
```

3. 修改本地yum源

```bash
# 备份
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bak
# 修改源文件
vi /etc/yum.repos.d/CentOS-Yum-Local.repo
```

修改内容如下：

```
[centos7-local]
name=Centos 7.6             ## 源名字
baseurl=file:///mnt/cdrom   ## 本地镜像文件路径  
enabled=1                   ## 1为启动yum源，0为禁用
gpgcheck=1                  ## 1为检查GPG-KEY，0为不检查
gpgkey=file:///mnt/cdrom/RPM-GPG-KEY-redhat-release   ##GPG-KEY文件路径
```

清除并更新缓存

```bash
yum clean all
yum makecache
# 安装工具
yum install net-tools
```

4. 也可以将iso文件上传到服务器，然后使用下面命令挂载：

```bash
mount -o loop /tmp/image.iso /mnt/cdrom
```
