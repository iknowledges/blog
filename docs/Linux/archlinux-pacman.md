# Arch Linux包管理器

## pacman

- 常用命令

```
# 更新系统包
pacman -Suy
# 查找包
pacman -Ss gcc
pacman -Sl | grep gcc
# 安装包
pacman -S core/gcc
# 删除包
pacman -R core/gcc
# 删除包及其依赖
pacman -Rs core/gcc
# 删除包和依赖以及配置文件
pacman -Rsn core/gcc
# 查询已安装带有gcc关键字的包
pacman -Qs gcc
# 查询包的依赖
pacman -Si boost
# 反向查询包的依赖
pacman -Sii boost
```

## yay（AUR助手）

- 安装

```
pacman -S yay
```

- 常用命令

```
# 查找包
yay -Ss cpprestsdk
# 安装包
yay -S cpprestsdk
# 刷新系统包并升级
yay -Syu
# 删除包
yay -Rns cpprestsdk
# 获取系统统计信息
yay -Ps
```

- 不使用AUR助手安装AUR软件包

在[AUR页面](https://aur.archlinux.org/)找到要安装的包，然后执行下面命令：

```
git clone [package URL]
cd [package name]
makepkg -si
```

#### 参考资料

- [初级：如何在Arch Linux中安装Yay AUR助手](https://linux.cn/article-14846-1.html)
- [pacman/Rosetta](https://wiki.archlinux.org/title/Pacman/Rosetta)
- [什么是Arch用户仓库（AUR）以及如何使用？](https://zhuanlan.zhihu.com/p/129855163)
