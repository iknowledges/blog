# Git子模块

## 获取子模块的两种方法

1. clone主模块时加上参数`--recursive`
2. clone主模块完成后进入主目录，执行下面命令：
```bash
git submodule init
git submodule update
```

## 创建子模块

```bash
git submodule add <submodule_url>
```

## 查看子模块

```
$ git submodule 
3f71a8aed2b72207688cd0e2c010a6d37f446220 pdf.js (vundefined-10575-g3f71a8aed)
```

## 完全删除子模块

```bash
# 执行后可发现模块目录被清空
git submodule deinit pdf.js
git rm pdf.js
```

## 删除子模块相关配置

1. 删除.gitmodules中记录的模块信息

```
git rm --cached pdf.js
```

2. 删除`.gitmodules`文件中相关子模块的信息

```
[submodule "pdf.js"]
	path = pdf.js
	url = https://github.com/zotero/pdf.js.git
	branch = reader1
```

3. 删除`.git/config`中相关子模块信息

```
[submodule]
	active = .

[submodule "pdf.js"]
	url = https://github.com/zotero/pdf.js.git
```

4. 删除`.git`文件夹中的相关子模块文件

```
rm -rf .git/modules/pdf.js/
```

## 修改url

1. 修改.gitmodules中的url
2. 执行下面命令：

```
git submodule sync
# 注册子模块
git submodule init
# 拉取子模块代码
git submodule update
```

## 更新子模块提交的代码

```
cd submodule_directory
git pull origin master
git log
git checkout 8b6eb6bf0d00b844cea03f283ebbf08f8b892cca
cd ..
git add submodule_directory
git commit -m "update submodule"
git push
```

#### 参考资料

[Git: submodule 子模块简明教程](https://zhuanlan.zhihu.com/p/404615843)
