# Ubuntu源码安装Python

1. 安装编译所需依赖

```
sudo apt update
sudo apt install build-essential zlib1g-dev libffi-dev libssl-dev libsqlite3-dev tk-dev
```

- zlib1g-dev: 解决`no module named zlib`
- libffi-dev: 解决`ssl module in python is not available`
- libssl-dev: 解决`no module named '_ctypes'`
- libsqlite3-dev: 解决`No module named '_sqlite3'`
- tk-dev: 解决`no module named '_tkinter'`

2. 下载Python源码

```
wget https://www.python.org/ftp/python/3.11.6/Python-3.11.6.tgz
tar zxvf Python-3.11.6.tgz
```

3. 编译安装

```
cd Python-3.11.6
./configure
make -j 4
sudo make install
```
