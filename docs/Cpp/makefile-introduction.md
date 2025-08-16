# An Introduction to Makefiles

## 2.1 What a Rule Looks Like

makefile组成规则如下：

```
target ... : prerequisites ...
        recipe
        ...
        ...
```

- target: 用于生成程序的文件名，通常是可执行文件或者对象文件；也可以是要执行的动作名称，如clean。
- prerequisite: 用于创建target的文件。
- recipe: 执行的动作。由一个或多个命令组成。注意每一个recipe行都要以一个tab字符开始。

## 2.2 A Simple Makefile

下面这个makefile描述了可执行文件edit依赖了八个对象文件，这八个对象文件又依赖了八个源文件和三个头文件：

```
edit : main.o kbd.o command.o display.o \
       insert.o search.o files.o utils.o
        cc -o edit main.o kbd.o command.o display.o \
                   insert.o search.o files.o utils.o

main.o : main.c defs.h
        cc -c main.c
kbd.o : kbd.c defs.h command.h
        cc -c kbd.c
command.o : command.c defs.h command.h
        cc -c command.c
display.o : display.c defs.h buffer.h
        cc -c display.c
insert.o : insert.c defs.h buffer.h
        cc -c insert.c
search.o : search.c defs.h buffer.h
        cc -c search.c
files.o : files.c defs.h buffer.h command.h
        cc -c files.c
utils.o : utils.c defs.h
        cc -c utils.c
clean :
        rm edit main.o kbd.o command.o display.o \
           insert.o search.o files.o utils.o
```

使用这个makefile生成可执行文件：

```
make
```

删除可执行文件和所有的对象文件：

```
make clean
```

## 2.4 Variables Make Makefiles Simpler

定义一个变量objects代表所有对象文件的名字，然后每个需要用到所有对象文件的地方都可以用变量的值$(objects)代替。

```
objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)
main.o : main.c defs.h
        cc -c main.c
kbd.o : kbd.c defs.h command.h
        cc -c kbd.c
command.o : command.c defs.h command.h
        cc -c command.c
display.o : display.c defs.h buffer.h
        cc -c display.c
insert.o : insert.c defs.h buffer.h
        cc -c insert.c
search.o : search.c defs.h buffer.h
        cc -c search.c
files.o : files.c defs.h buffer.h command.h
        cc -c files.c
utils.o : utils.c defs.h
        cc -c utils.c
clean :
        rm edit $(objects)
```

## 2.5 Letting make Deduce the Recipes

不需要详细写出编译每个C源文件的方法，因为make可以自己计算出来：有一条隐含的规则是使用"cc -c"命令从相应的".c "文件更新".o"文件。

```
objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean
clean :
        rm edit $(objects)
```

## 2.6 Another Style of Makefile

运用了隐式规则的makefile将是另一种风格，你可以按照prerequisites而不是target进行分组：

```
objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)

$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h
```

## 2.7 Rules for Cleaning the Directory

```
.PHONY : clean
clean :
        -rm edit $(objects)
```

这样写防止了make被一个名为clean的真实文件所迷惑，并能在rm出错时继续运行。

#### 参考资源

- [GNU make](https://www.gnu.org/software/make/manual/make.html)