# Velocity模板语法

## 常用符号

- `#`用来标识Velocity的指令，如`#set`、`#if`、`#else`、`#end`、`#foreach`、`#end`、`#include`、`#parse`、`#macro`等。

- `$`用来标识Velocity的变量，如`$i`、`$msg`、`$TagUtil.options(...)`等。

- `{}`用来明确标识Velocity变量，比如在页面中有一个`$someonename`，此时Velocity将把someonename作为变量名，若我们想在someone这个变量的后面紧接着显示name字符，则应该改成`${someone}name`。

- `!`用来强制把不存在的变量显示为空白，比如当找不到username的时候，`$username`返回字符串"`$username`"，而`$!username`返回空字符串""。

## 常用指令

### Set

set指令可以给变量赋值，也可以给属性赋值
```
#set( $primate = "monkey" )
#set( $customer.Behavior = $primate )
```

其他类型
```
#set( $monkey = $bill ) ## variable reference
#set( $monkey.Friend = "monica" ) ## string literal
#set( $monkey.Blame = $whitehouse.Leak ) ## property reference
#set( $monkey.Plan = $spindoctor.weave($web) ) ## method reference
#set( $monkey.Number = 123 ) ##number literal
#set( $monkey.Say = ["Not", $my, "fault"] ) ## ArrayList
#set( $monkey.Map = {"banana" : "good", "roast beef" : "bad"}) ## Map
```

### 条件语句

1. If / ElseIf / Else

```
#if( $foo < 10 )
    **Go North**
#elseif( $foo == 10 )
    **Go East**
#elseif( $bar == 6 )
    **Go South**
#else
    **Go West**
#end
```

2. 逻辑操作符

```
## logical AND

#if( $foo && $bar )
  ** This AND that**
#end

## logical OR

#if( $foo || $bar )
    **This OR That**
#end

##logical NOT

#if( !$foo )
  **NOT that**
#end
```

### 循环

遍历Array
```
<ul>
#foreach( $product in $allProducts )
    <li>$product</li>
#end
</ul>
```
遍历Hashtable
```
<ul>
#foreach( $key in $allProducts.keySet() )
    <li>Key: $key -> Value: $allProducts.get($key)</li>
#end
</ul>
```
`count`获取循环次数
```
<table>
#foreach( $customer in $customerList )
    <tr><td>$foreach.count</td><td>$customer.Name</td></tr>
#end
</table>
```
`hasNext`判断是否是最后一次循环
```
#foreach( $customer in $customerList )
    $customer.Name#if( $foreach.hasNext ),#end
#end
```
`break`跳出循环
```
## list first 5 customers only
#foreach( $customer in $customerList )
    #if( $foreach.count > 5 )
        #break
    #end
    $customer.Name
#end
```

### `#parse`和`#include`

`#parse`和`#include`指令的功能都是在外部引用文件，而两者的区别是：`#parse`会将引用的内容当成类似于源码文件，会将内容在引入的地方进行解析；`#include`是将引入文件当成资源文件，会将引入内容原封不动地以文本输出。分别看以下例子：

foo.vm文件：
```
#set($name = "velocity")
```

parse.vm：
```
#parse("foo.vm")
```
输出结果为：velocity

include.vm：
```
#include("foo.vm")
```
输出结果为：#set($name = "velocity")

### 宏

Velocity中的宏可以理解为函数定义。其语法如下：
```
#macro(macroName arg1 arg2 ...)
...
#end
```
调用这个宏的语法是：
```
#macroName(arg1 arg2 ...)
```
这里的参数之间使用空格隔开，下面是定义和使用Velocity宏的例子：
```
#macro(sayHello $name)
    hello $name
#end

#sayHello("velocity")
```
输出的结果为hello velocity

### 注释

单行注释
```
## 单行注释
```
多行注释
```
#*
    多行注释
*#
```

### 单双引号

单引号不解析引用内容，双引号解析引用内容
```
#set ($var="hello")

'$var'  ## 结果为：$var
"$var"  ## 结果为：hello
```

## 参考资料

[Velocity模板引擎语法 ](https://www.cnblogs.com/yangzhinian/p/4885973.html)
