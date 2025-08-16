# C++处理中文字符串

C++中通过下标访问字符串的位数时表示的是第几个字节，并不是字符。我们在遍历英文字符串时不会有问题，但是遍历中文字符串时会出现乱码。

```cpp
std::string utf8_string = "Hello,《你好》";
for (size_t i = 0; i < utf8_string.size(); i++) {
    char ch = utf8_string[i];
    std::cout << ch << std::endl;
}
```

处理中文字符串的方法是转换成宽字符串：

```cpp
#include <iostream>
#include <string>
#include <locale>
#include <codecvt>

int main() {
    // 示例UTF-8字符串
    std::string utf8_string = "Hello,《你好》";

    std::wstring_convert<std::codecvt_utf8<wchar_t>> converter;
    // 将string转为wstring
    std::wstring wide_string = converter.from_bytes(utf8_string);
    
    // 下面两行代码对地区环境进行了初始化，空字符串表示使用环境中缺省的locale
    // 设置C++标准IO库的全局locale
    std::locale::global(std::locale(""));
    // 设置wcout的locale
    std::wcout.imbue(std::locale(""));

    // 遍历宽字符串
    for (size_t i = 0; i < wide_string.size(); i++) {
        wchar_t wc = wide_string[i];
        std::wcout << i + 1 << L": " << wc << std::endl;
    }

    // 将wstring转为string
    std::string original_string = converter.to_bytes(wide_string);
    // 重新打开输出流，不建议将cout和wcout混合使用
    freopen(nullptr, "a", stdout);
    std::cout << original_string << std::endl;
    return 0;
}
```

#### 参考资料

- [[C++] cout、wcout无法正常输出中文字符问题的深入调查（1）：各种编译器测试](https://www.cnblogs.com/zyl910/archive/2013/01/20/wchar_crtbug_01.html)
