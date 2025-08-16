# conan2使用教程

安装和卸载conan

```bash
pip install conan
pip uninstall conan
rm -rf ~/.conan2/
```

在CMakeLists.txt相同的路径下创建conanfile.txt

```bash
[requires]
boost/1.79.0

[generators]
CMakeDeps
CMakeToolchain
```

然后生成debug的profile文件并安装conanfile.txt中的依赖

```bash
conan profile detect debug
conan install . --output-folder=cmake-build-debug --build=missing --profile=debug
```

在cmake中使用conan安装的依赖

```bash
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Debug
```

其他conan命令

```bash
# 列出所有已安装的依赖
conan list '*'
# 删除指定依赖
conan remove boost/1.79.0
# 查看依赖的安装路径
conan cache path boost/1.79.0
# 从远程仓库搜索
conan search boost --remote=conancenter
# 查看远程仓库地址
conan remote list
```

clion配置的Build type要和profile中的一致

![clion](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Clion/conan2.png)