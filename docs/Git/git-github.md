# 本地上传代码到github仓库

#### 一、ssh远程设置

1. 进入本地代码目录，打开git bash，输入：
```bash
git config --global user.name "aaa" 
git config --global user.email "aaa@163.com" 
ssh-keygen -t rsa -C "yourname@email.com"
# 删除某项配置
git config --global --unset user.name
```
默认在/c/Users/Administrator/.ssh/id_rsa目录下生成以下文件

![1](https://gitlab.com/iknowledge/BlogImage/-/raw/main/GitHub/github01.png)

2. 登入你的github账号，在Settings->SSH and GPG keys下new一个SSH key，将id_rsa.pub中的内容拷贝过来就行了，title自定义。

![2](https://gitlab.com/iknowledge/BlogImage/-/raw/main/GitHub/github02.png)

3. 输入以下命令验证ssh是否生效

```bash
ssh -T git@github.com
```
![3](https://gitlab.com/iknowledge/BlogImage/-/raw/main/GitHub/github03.png)

#### 二、初次建立本地仓库并提交代码

1. 首先建立本地仓库
```bash
git init
```

2. 将文件添加到仓库
```bash
git add README.md //添加文件
git add . //添加当前目录
```

3. 把文件提交到仓库
```bash
git commit -m "first commit"
```

4. 关联本地仓库，仓库地址通过图示位置复制过来
```bash
git remote add origin git@github.com:<仓库地址>
```

![4](https://gitlab.com/iknowledge/BlogImage/-/raw/main/GitHub/github04.png)

5. 把本地库的所有内容推送到远程库上
```bash
git push -u origin master
```

#### 三、同一主机更新代码

1. 同步github仓库
```bash
git pull origin master
```

2. 同步之后就可以开始上次更新了，.是包含目录下所以文件，也可以指定具体文件名。
```bash
git add .
```

3. 提交信息可以换成你想要的指令
```bash
git commit  -m  "提交信息"
```

4. 最后把本地仓库push到github上
```bash
git push -u origin master 
```

#### 四、另一主机更新代码

1. 拉取仓库代码到本地
```bash
git clone git@github.com:<仓库地址>
```

2. 设置提交时的用户
```bash
git config --global user.name "aaa" 
git config --global user.email "bbb@163.com" 
```

3. 同步github仓库
```bash
git pull origin master
```

4. 提交代码
```bash
git add .
git commit  -m  "提交信息"
git push -u origin master 
```
