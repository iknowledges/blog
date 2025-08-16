# Google Performance Tools性能分析

1. Ubuntu安装工具

```
sudo apt install google-perftools libgoogle-perftools-dev
```

2. 编写测试代码main.cpp

```cpp
#include <iostream>
#include <gperftools/profiler.h>

long long fibonacci(int n) {
    if (n <= 1)
        return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    ProfilerStart("profile.prof"); // 启动性能分析

    int n = 40;
    std::cout << "Fibonacci of " << n << " is " << fibonacci(n) << std::endl;

    ProfilerStop(); // 停止性能分析
    return 0;
}
```

3. 编译并执行程序

```
g++ main.cpp -lprofiler -o main.out
./main.out
```

4. 查看分析结果

```
# 直接查看结果
pprof --text ./main.out profile.prof
# 将结果输出到pdf
pprof --pdf ./main.out profile.prof > output.pdf
```

#### 问题解决

- 问题1：没有找到pprof工具

执行下面命令找到工具路径为：/usr/bin/google-pprof，用google-pprof代替pprof即可

```
dpkg -L google-perftools | grep pprof
```

#### 参考资料

- [cpuprofile](https://gperftools.github.io/gperftools/cpuprofile.html)
- [使用gperftools分析程序性能](https://luyuhuang.tech/2022/04/10/gperftools.html)
