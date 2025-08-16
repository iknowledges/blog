# C++的lambda表达式

```cpp
auto func = [capture] (params) opt -> ret { func_body; };
```
1. func是可以当作lambda表达式的函数名字；
2. capture是捕获列表；

- []不捕获任何变量
- [&]引用捕获，捕获外部作用域所有变量，在函数体内当作引用使用
- [=]值捕获，捕获外部作用域所有变量，在函数内有个副本使用
- [=, &a]值捕获外部作用域所有变量，按引用捕获a变量
- [a]只值捕获a变量，不捕获其它变量
- [this]捕获当前类中的this指针

3. params是参数列表；
4. opt是函数选项(mutable之类)；
5. ret是返回值类型；
6. func_body是函数体。

示例：
```cpp
auto func1 = [](int a) -> int { return a + 1; };
// c++11允许省略表达式的返回值定义
auto func2 = [](int a) { return a + 2; };

int a = 0;
auto f1 = [=]() { return a; }; // 值捕获a
auto f2 = [=]() { return a++; }; // 修改按值捕获的外部变量，error
auto f3 = [=]() mutable { return a++; };  // mutable去掉const属性，可以修改a
```
