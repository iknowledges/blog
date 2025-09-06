# 安装Rust

## Ubuntu

1. 配置源，在~/.bashrc下加入如下内容：

```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

2. 安装

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

3. 验证是否安装成功

```
rustc --version
```

4. 卸载Rust

```
rustup self uninstall
```

## Windows

1. 设置环境变量，自定义Rust的安装路径：

- RUSTUP_HOME: `D:\Software\Rust\.rustup`
- CARGO_HOME: `D:\Software\Rust\.cargo`

2. 下载[rustup-init.exe](https://www.rust-lang.org/tools/install)，然后打开powershell执行下面命令进行安装：

```
$env:RUSTUP_DIST_SERVER="https://mirrors.ustc.edu.cn/rust-static"
$env:RUSTUP_UPDATE_ROOT="https://mirrors.ustc.edu.cn/rust-static/rustup"
.\rustup-init.exe
```

3. 其他命令

```
# 查看版本
rustc --version
# 卸载Rust
rustup self uninstall
```

#### 参考资源

- [Install Rust](https://www.rust-lang.org/tools/install)
- [RUST安装慢怎么办，使用镜像方式安装](https://blog.csdn.net/u010964076/article/details/105200376)
