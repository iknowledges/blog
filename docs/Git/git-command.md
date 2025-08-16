# Git入门操作

#### 创建git仓库:
```bash
mkdir book-notes
cd book-notes
# 把这个目录变成Git可以管理的仓库
git init
# 新建文件并添加
touch README.md
git add README.md
# 把文件提交到仓库
git commit -m "first commit"
# 关联远程仓库
git remote add origin https://<仓库地址>
# 把本地库的内容推送到远程仓库
git push -u origin master
```

#### 设置镜像加速
```bash
# 设置
git config --global url."https://hub.yzuu.cf".insteadOf https://github.com
# 取消设置
git config --global --unset url."https://hub.yzuu.cf".insteadOf
# 查看配置
git config --global --list
```

镜像地址可在浏览器安装【Github增强】插件获取
```
https://hub.yzuu.cf
https://hub.njuu.cf
```

#### 分支管理

##### branch

```bash
# 列出本地分支
git branch
# 创建本地分支testing
git branch testing
# 删除本地分支testing
git branch -d testing
# 删除远程分支
git push origin :testing
```

##### tag

```bash
# 查看tag
git tag -l
# 删除本地tag
git tag -d <tag_name>
# 删除远程tag
git push origin :refs/tags/<tag_name>
```

##### checkout

```bash
# 切换到testing分支
git checkout testing
# 本地新建testing分支并切换到该分支
git checkout -b testing
# 查看修改内容
git diff
# 回滚文件
git checkout HEAD -- hello.cpp
# 回滚目录下的所有文件
git checkout HEAD -- test/
```

##### merge

```bash
# 将testing分支合并到当前分支
git merge testing
```

#### reset

命令语法格式：

```
git reset [--soft | --mixed | --hard] [HEAD]
```

- --mixed为默认，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。
- --soft参数用于回退到某个版本。
- --hard参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交，谨慎使用！

HEAD说明：

- 使用^：
  - HEAD 表示当前版本
  - HEAD^ 上一个版本
  - HEAD^^ 上上一个版本
  - HEAD^^^ 上上上一个版本
- 使用~数字：
  - HEAD~0 表示当前版本
  - HEAD~1 上一个版本
  - HEAD~2 上上一个版本
  - HEAD~3 上上上一个版本

示例：

```bash
# 回滚所有内容到上一个版本
git reset HEAD^
# 回滚指定文件到上一个版本
git reset HEAD^ hello.cpp
# 回滚到上上一个版本
git reset HEAD~2
# 回滚到指定版本
git reset <commit-id>
# 强制回滚到某次提交（必须有强制推送权限）
git reset --hard <commit-id>
git push -f origin master
```

Gitlab上设置强制提交权限：Settings->Repository->Protected branches->Allowed to force push

#### revert

```bash
# 撤销某次提交，弹出界面输入:wq保存
git revert <commit-id>
# 如果强制关闭弹出界面，就需要重新commit
git commit -m 'revert'
# 推送到远程仓库
git push
# 没push前如果想删除revert提交，可以使用reset
# 注意：reset和revert的commit-id要一致
git reset <commit-id>
```

#### remote

命令语法格式：
```
git remote add [name] [url]
git remote remove [name]
```
- name为本地的仓库别名，如orgin
- url为远程仓库链接地址

示例：
```bash
# 显示所有远程仓库：仓库别名name+url
git remote -v
# 显示某个远程仓库的信息
git remote show https://<仓库地址>
# 添加远程仓库并提交代码
git remote add origin https://<仓库地址>
git push -u origin master
# 删除本地关联的远程仓库
git remote remove origin
```

#### cherry-pick

命令语法格式：
```
git cherry-pick <commit-id>
```

如仓库有master和feature两个分支，现将feature上的某次提交同步到master分支
```
# 在feature分支上查看提交日志
git log
# 切换到master分支合并
git checkout master
git cherry-pick <commit-id>
```

#### 其他命令

```bash
# 查看提交日志
git log
# 查看当前代码是否有更新
git status
# --cached表示文件从暂存区域移除，但工作区保留
# -r表示递归删除
git rm -r --cached .
```

#### 参考资料

[无需代理直接加速各种GitHub资源拉取](https://zhuanlan.zhihu.com/p/463954956)
[git cherry-pick教程](https://ruanyifeng.com/blog/2020/04/git-cherry-pick.html)
[git push之后回滚（撤销）代码](https://blog.csdn.net/qq_36460164/article/details/79857431)
