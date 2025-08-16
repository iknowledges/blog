# 使用shared_from_this时出现bad_weak_ptr的原因

1. 在构造函数中调用shared_from_this()；
2. 调用shared_from_this()的类不是以智能指针的形式初始化对象的；
3. 不是public继承。

#### 参考资料

- [shared_from_this使用中运行错误bad_weak_ptr原因分析](https://blog.csdn.net/xhtchina/article/details/126296962)
