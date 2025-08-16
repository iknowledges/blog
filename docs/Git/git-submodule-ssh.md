# Git使用SSH更新子模块

## 方法一：修改.gitmodules文件

1. 拉取代码仓库

```
git clone -b v1.64.0 git@github.com:grpc/grpc
cd grpc
```

2. 将.gitmodules文件中的url由`https://github.com/`改为`git@github.com:`，如下：

```
[submodule "third_party/abseil-cpp"]
	path = third_party/abseil-cpp
	url = git@github.com:abseil/abseil-cpp.git
```

3. 更新子模块

```
# 同步.gitmodules文件和子模块config配置中的url
git submodule sync
# 拉取子模块代码，第一次拉取时要加--init
git submodule update --init
```

## 方法二：设置URL别名

命令格式：

```
git config --global url.<base>.insteadOf <extra>
```

示例说明：

```
# 将"git@github.com:"的别名设置为"https://github.com/"
git config --global url."git@github.com:".insteadOf "https://github.com/"
# 查看是否生效
git config --global --list
# 克隆仓库
git clone --recurse-submodules -b v1.64.0 --depth 1 --shallow-submodules https://github.com/grpc/grpc
# 取消设置
git config --global --unset url."git@github.com:".insteadOf
```

#### 参考资料

- [Git自动通过SSH或HTTPS访问Git子模块](https://geek-docs.com/git/git-questions/1988_git_automatically_access_git_submodules_via_ssh_or_https.html)
- [Git 如何打破 git config .insteadOf 的链接](https://deepinout.com/git/git-questions/218_git_how_can_i_break_the_link_of_git_config_urlinsteadof_url.html)
