# pybind11示例

源码地址：https://github.com/tdegeus/pybind11_examples

1. 克隆pybind11项目，速度慢可以使用代理地址hub.yzuu.cf或hub.njuu.cf

```shell
git clone https://github.com/pybind/pybind11.git
```

2. 创建软链接

```shell
cd 01_py-list_cpp-vector/
ln -s ../pybind11/ pybind11
```

3. 编译

```shell
mkdir build
cd build/
# PYTHON_EXECUTABLE指定python解释器
cmake .. -DPYTHON_EXECUTABLE=/usr/bin/python3
make
```

4. 测试

```shell
mv example.cpython-36m-x86_64-linux-gnu.so ../example.so
cd ..
python test.py
```
