# OpenCV在Ubuntu源码安装教程

1. 安装系统包

```bash
sudo apt install pkg-config libgtk2.0-dev
```

2. 下载源码

```bash
mkdir opencv4
git clone https://github.com/opencv/opencv.git
cd opencv/
git checkout 4.5.5
cd ..
git clone https://github.com/opencv/opencv_contrib.git
cd opencv_contrib/
git checkout 4.5.5
cd ..
```

3. 编译安装

```bash
cd opencv
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/roy/software/opencv4/opencv -DOPENCV_EXTRA_MODULES_PATH=/home/roy/software/opencv4/opencv_contrib/modules ..
make -j4
make install
```

4. 设置环境变量，OpenCV_DIR必须是包含OpenCVConfig.cmake的路径。

```bash
export OpenCV_DIR=/home/roy/software/opencv4/opencv/build
```

## 参考资料

[install opencv on ubuntu](https://learnopencv.com/install-opencv-3-4-4-on-ubuntu-18-04/)