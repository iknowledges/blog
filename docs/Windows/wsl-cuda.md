# WSL安装CUDA

## 安装NVIDIA驱动

1. 打开[NVIDIA官网](https://www.nvidia.com/en-us/drivers/)，选择如下选项，然后下载Game Ready驱动：

- Select Product Category: GeForce
- Select Product Series: GeForce RTX 30 Series
- Select Product: GeForce RTX 3060
- Select Operating System: Windows 11
- Select Language: English (US)

2. 打开命令行，执行如下命令：

```
nvidia-smi
```

显示如下内容：

```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 596.36                 Driver Version: 596.36         CUDA Version: 13.2     |
+-----------------------------------------+------------------------+----------------------+
```

## 安装CUDA

1. 上一步显示的【CUDA Version】就是显卡驱动支持的CUDA版本，这里要注意【NVIDIA-SMI】和【Driver Version】的版本是否一致，因为有时自动更新后可能更新了【Driver Version】，但是【NVIDIA-SMI】还是老版本，这时要以低版本为准，然后查阅[Table 3 CUDA Toolkit and Corresponding Driver Versions](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)找到对应的CUDA版本。

2. 打开[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)，选择【CUDA Toolkit 13.2.1】，然后依次选择【Linux】->【x86_64】->【WSL-Ubuntu】->【2.0】->【runfile (local)】：

```
wget https://developer.download.nvidia.com/compute/cuda/13.2.1/local_installers/cuda_13.2.1_595.58.03_linux.run
sudo sh cuda_13.2.1_595.58.03_linux.run
```

默认安装选项如下：
- [X] CUDA Toolkit 13.2
- [X] CUDA Documentation 13.2

注意：这里推荐选择runfile进行安装，因为runfile会生成卸载命令cuda-uninstaller，而使用apt安装不会生成。

5. 设置环境变量，编辑`~/.bashrc`：

```
export CUDA_HOME=/usr/local/cuda-13.2
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

6. 查看CUDA版本

```
source ~/.bashrc
nvcc -V
```

## 卸载

1. 参照文档[CUDA Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-uninstallation)

- runfile安装cuda后的卸载命令是：

```
sudo /usr/local/cuda-13.2/bin/cuda-uninstaller
```

- apt安装cuda后的卸载命令是：

```
# 这里的package_name各个版本安装的名称不一样
sudo apt --purge remove <package_name>
sudo apt autoremove
```

2. 删除`~/.bashrc`中的相关配置，并确认`/usr/local/`目录下cuda相关文件已删除。

#### 参考资源

- [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)
- [13. Post-installation Actions](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
