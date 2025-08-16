# KVM安装MacOS

1. 克隆项目

```
cd ~
git clone --depth 1 --recursive https://github.com/kholia/OSX-KVM.git
cd OSX-KVM
```

2. 选择1-4下载相应安装介质，默认名称为BaseSystem.dmg

```
$ ./fetch-macOS-v2.py
1. High Sierra (10.13)
2. Mojave (10.14)
3. Catalina (10.15)
4. Big Sur (11.7)
5. Monterey (12.6)
6. Ventura (13) - RECOMMENDED

Choose a product to download (1-6): 4
```

3. 将下载的安装介质转换成可用的格式

```
dmg2img -i BaseSystem.dmg BaseSystem.img
```

4. 创建一个目标磁盘文件用于安装

```
qemu-img create -f qcow2 mac_hdd_ng.img 128G
```

5. 开始安装

修改脚本参数：

```
# ALLOCATED_RAM设置内存大小，默认单位为128MB，支持M、G为单位
-m "$ALLOCATED_RAM"
# CPU_THREADS表示逻辑CPU数量
# CPU_SOCKETS表示socket数量
# CPU_CORES表示每个socket的核心数量
# [,threads=threads]设置每个核心的线程数
-smp "$CPU_THREADS",cores="$CPU_CORES",sockets="$CPU_SOCKETS"
```

然后运行脚本：

```
./OpenCore-Boot.sh
```

#### 参考资料

- [OSX-KVM](https://github.com/kholia/OSX-KVM)
- [macOS-Simple-KVM](https://github.com/foxlet/macOS-Simple-KVM)
- [OSX-KVM: 在KVM虚拟环境运行macOS](https://cloud-atlas.readthedocs.io/zh_CN/latest/kvm/kvm_macos/osx_kvm.html)
- [KVM上安装MacOSX到底难不难](https://zhuanlan.zhihu.com/p/474047948)
- [OSX-KVM安装备忘指南](https://www.cnblogs.com/b-sir/p/13265722.html)
