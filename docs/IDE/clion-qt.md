# Clion配置Qt开发环境

1. 【Toolchains】添加【Visual Studio】，【Toolset】设置为`D:\Program Files (x86)\Microsoft Visual Studio\2022\Community`。
2. 【CMake】中【CMake options】添加`-DCMAKE_PREFIX_PATH=D:/Software/Qt/6.8.3/msvc2022_64`。
3. 【Run/Debug Configurations】->【CMake Application】->【Environment variables】添加`Path=D:\Software\Qt\6.8.3\msvc2022_64\bin`。
