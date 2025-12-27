# Ubuntu命令笔记

#### sed

```bash
# 将demo_file文件中的str1替换为str2，g代表全局
sed -i s/str1/str2/g demo_file

# 匹配字符串str1并在上一行添加str2
sed -i '/str1/i\str2' demo_file
 
# 匹配字符串str1并在下一行添加str2
sed -i '/str1/a\str2' demo_file
```

#### chpasswd

```bash
# 批量或单一修改用户密码
echo <用户名>:<密码> | chpasswd
```

#### useradd
```bash
# 创建admin用户
useradd -r -m -s /bin/bash admin
```
- -r：建立系统账号
- -m：自动建立用户的登入目录
- -s：指定用户登入后所使用的shell

#### adduser
```bash
# 将用户admin添加到sudo用户组
adduser admin sudo
```

#### df

```bash
# 查看磁盘空间使用情况
df -h
```

#### du

- -c, --total 累计大小
- -d, --max-depth=N 决定统计每个目录的深度
- -B, --block-size=SIZE 决定显示文件大小的单位;比如 ‘-BM’，就是MB，'-BK’就是KB
- -h, --human-readable 以高可读方式打印 (比如1K 234M 2G)
- -s, --summarize 显示总大小

```bash
# 当前目录下文件夹大小
du -sh *
# 当前目录下文件夹大小（包括隐藏文件）
du -sh * .[^.]*
```

#### dpkg

```
# 列出google相关软件
dpkg -l google*
# 通过deb文件安装软件
dpkg -i *.deb
# 删除安装的软件，不删除配置文件
dpkg --remove google-chrome-stable
# 删除安装的软件及配置文件
dpkg --purge google-chrome-stable
```

#### apt

```
# 只卸载包
sudo apt-get remove libgrpc++-dev
# 卸载包及其依赖
sudo apt-get autoremove libgrpc++-dev
# 移除配置和数据文件
sudo apt-get purge libgrpc++-dev
# 移除配置、数据文件，以及依赖
sudo apt-get autoremove --purge libgrpc++-dev
```

#### add-apt-repository

PPA的网址：https://launchpad.net/

```
# 添加PPA
sudo add-apt-repository ppa:PPA_Name/ppa
sudo apt update
# 删除PPA
sudo add-apt-repository --remove ppa:PPA_Name/ppa
```

#### nohup

语法格式：nohup Command [ Arg… ] [ & ]

- Command：要执行的命令。
- Arg：一些参数，可以指定输出文件。
- &：让命令在后台执行，终端退出后命令仍旧执行。

```
# 只输出错误到日志文件log
nohup /home/run.sh >/dev/null 2>log &
# 什么信息都不输出
nohup /home/run.sh >/dev/null 2>&1 &
# 按日期输出日志
nohup /home/run.sh >> my_`date +%Y-%m-%d`.log &
```

2>&1解释：

将标准错误2重定向到标准输出&1，标准输出&1再被重定向输入到/dev/null。

- 0 - stdin (standard input，标准输入)
- 1 - stdout (standard output，标准输出)
- 2 - stderr (standard error，标准错误输出)

#### tar

- 压缩

```
# 打包成.tar文件
tar -cvf [文件名].tar [文件目录]
# 打包成.bz2文件
tar -jcvf [文件名].tar.bz2 [文件目录]
# 打包成.gz文件
tar -zcvf [文件名].tar.gz [文件目录]
```

- 解压缩

```
# 解压.tar文件到指定目录
tar -xvf [文件名].tar -C [文件目录]
# 解压.bz2文件到指定目录
tar -jxvf [文件名].tar.bz2 -C [文件目录]
# 解压.gz文件到指定目录
tar -zxvf [文件名].tar.gz -C [文件目录]
```

#### find

查找/usr目录中后缀为.cmake的文件：

```
find /usr -type f -name "*.cmake"
```

- -type f：用于指定查找文件；
- -name "*.cmake"：指定匹配以.cmake结尾的文件名。

#### ps

```
# 查看特定进程的CPU使用情况
ps -p <PID> -o pid,comm,%cpu,%mem
```