# Python中星号(*)和双星号(**)的用法

## 定义函数

- 单星号代表收集参数

```python
def myprint(*params):
    print(params)

myprint(1, 2, 3)
# 打印出 (1, 2, 3)
```

- 双星号代表收集关键字参数

```python
def myprint(**params):
    print(params)

myprint(x=1, y=2, z=3)
# 打印出 {'x': 1, 'y': 2, 'z': 3}
```

## 函数调用

- 单星号代表分配参数

```python
def myprint(x,y):
    print(x)
    print(y)

params=(1, 2)
myprint(*param)
```

- 双星号代表分配关键字参数

```python
def myprint(x,y):
    print(x)
    print(y)

params={'x': 1,'y': 2}
myprint(**param)
```

#### 参考资源

- [Python中的*（星号）和**（双星号）完全详解](https://blog.csdn.net/zkk9527/article/details/88675129)