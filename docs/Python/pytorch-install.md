# Pytorch安装教程

## 安装CUDA

1. 打开【NVIDIA控制面板】->【系统信息】，查看【驱动程序版本】，如我的是`560.94`。

2. 打开官方文档[Table 3 CUDA Toolkit and Corresponding Driver Versions](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)，查看CUDA和NVIDIA显卡驱动版本对照表，发现我的驱动支持CUDA的最高版本为`CUDA 12.6 Update 2`。

3. 在官网下载[CUDA Toolkit 12.6.2](https://developer.nvidia.com/cuda-toolkit-archive)，双击安装程序`cuda_12.6.2_560.94_windows.exe`。首先会弹出一个选择解压临时文件夹的提示，默认就行，安装选项选择【自定义】，只要选择`CUDA`，取消勾选如下选项：`NVIDIA GeForce Experience`、`Other components`、`Driver components`，以及`CUDA`选项中的`Visual Studio Integration`。

4. 测试是否安装成功：

```
nvcc --version
```

## 安装Pytorch

1. 输入如下命令进行安装，如果速度太慢，可以直接下载[torch-2.9.1+cu126-cp312-cp312-win_amd64.whl](https://download.pytorch.org/whl/torch/)再进行安装。

```
pip install torch --index-url https://download.pytorch.org/whl/cu126
```

2. 测试是否安装成功：

```python
import torch
torch.cuda.is_available()
```

#### 参考资料

- [Start Locally](https://pytorch.org/get-started/locally/)
- [CUDA Zone](https://developer.nvidia.com/cuda-zone)
- [各GPU支持的CUDA版本gpu cuda支持列表](https://blog.51cto.com/u_16213611/10480090)