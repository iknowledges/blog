# C++中push_back和emplace_back的区别

```cpp
#include <iostream>
#include <string>
#include <vector>

class Person {
public:
    Person() {
        std::cout << "无参构造函数" << std::endl;
    }

    Person(const std::string& name, int age) : name(name), age(age) {
        std::cout << "有参构造函数" << std::endl;
    }

    Person(const Person& other) {
        std::cout << "拷贝构造函数" << std::endl;
        name = other.name;
        age = other.age;
    }

    Person(const Person&& other) {
        std::cout << "移动构造函数" << std::endl;
        name = other.name;
        age = other.age;
    }

    Person& operator=(const Person& other) {
        std::cout << "赋值操作函数" << std::endl;
        this->name = other.name;
        this->age = other.age;
        return *this;
    }

    ~Person() {
        std::cout << "析构函数" << std::endl;
    }

    std::string name;
    int age;
};

int main() {
    std::vector<Person> vec;

    Person p("张三", 18);
    // push左值：只调拷贝构造
    std::cout << "-----push lvalue start-----" << std::endl;
    vec.push_back(p);
    std::cout << "-----push lvalue end-----" << std::endl << std::endl;

    // emplace左值：只调拷贝构造
    std::cout << "-----emplace lvalue start-----" << std::endl;
    vec.emplace_back(p);
    std::cout << "-----emplace lvalue end-----" << std::endl << std::endl;

    // push右值：先调有参构造，再调移动构造，然后还会调析构函数
    std::cout << "-----push rvalue start-----" << std::endl;
    vec.push_back(Person("李四", 19));
    std::cout << "-----push rvalue end-----" << std::endl << std::endl;

    // emplace右值：先调有参构造，再调移动构造，然后还会调析构函数
    std::cout << "-----emplace rvalue start-----" << std::endl;
    vec.emplace_back(Person("李四", 19));
    std::cout << "-----emplace rvalue end-----" << std::endl << std::endl;

    // emplace参数（参数也是右值）：只调有参构造
    std::cout << "-----emplace parameter start-----" << std::endl;
    vec.emplace_back("李四", 19);
    std::cout << "-----emplace parameter end-----" << std::endl;
    return 0;
}
```