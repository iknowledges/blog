# Ubuntu安装build-essential报错

## Ubuntu20.04

报错信息：

```
下列软件包有未满足的依赖关系：
 build-essential : 依赖: libc6-dev 但是它将不会被安装 或
                           libc-dev
                   依赖: g++ (>= 4:9.2) 但是它将不会被安装
E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系
```

把 libc6:amd64 从 2.31-0ubuntu9.9 降级到 2.31-0ubuntu9.7

```
sudo apt install --reinstall libc6=2.31-0ubuntu9.7
```

## Ubuntu22.04

报错信息：

```
下列软件包有未满足的依赖关系：
 libc6-dev : 依赖: libc6 (= 2.35-0ubuntu3) 但是 2.35-0ubuntu3.1 正要被安装
E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。
```

查找依赖信息：

```
$ sudo apt list | grep libc6
...
libc6/now 2.35-0ubuntu3.1 amd64 [已安装，本地]
...
```

重新安装依赖：

```
sudo apt install --reinstall libc6=2.35-0ubuntu3
```

然后重新安装：

```
sudo apt install build-essential
```

