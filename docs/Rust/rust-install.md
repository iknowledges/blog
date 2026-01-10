# 安装Rust

## Ubuntu

1. 在`~/.bashrc`中添加如下内容设置安装目录，注意路径不要以`/`结束：

```
# 默认安装目录
export RUSTUP_HOME=~/.rustup
export CARGO_HOME=~/.cargo
```

2. 在安装rustup前，先设置环境变量，`RUSTUP_DIST_SERVER`用于更新toolchain，`RUSTUP_UPDATE_ROOT`用于更新rustup：

```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

3. 安装

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

4. 验证是否安装成功

```
rustc --version
```

5. 卸载Rust

```
rustup self uninstall
```

6. 设置Rust Crates Registry源，新建`$CARGO_HOME/config.toml`配置文件，并添加如下内容：

```
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"

[registries.ustc]
index = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

## Windows

1. 设置环境变量，自定义Rust的安装路径：

- RUSTUP_HOME: `D:\Rust\windows\.rustup`
- CARGO_HOME: `D:\Rust\windows\.cargo`

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
- [Rust Toolchain 反向代理](https://mirrors.ustc.edu.cn/help/rust-static.html)
- [Rust Crates](https://mirrors.ustc.edu.cn/help/crates.io-index.html)
