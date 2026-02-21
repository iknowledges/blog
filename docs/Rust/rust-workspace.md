# Rust Workspace教程

1. 创建workspace目录：

```
mkdir rust_workspace_example
cd rust_workspace_example
```

2. 新建Cargo.toml文件：

```
[workspace]
resolver = "3"
```

参考[Resolver versions](https://doc.rust-lang.org/cargo/reference/resolver.html#resolver-versions)，resolver含义如下：

- "1" (default)
- "2" (edition = "2021" default)
- "3" (edition = "2024" default, requires Rust 1.84+)

2. 在workspace中创建两个Rust项目：

```
cargo new crate_a
cargo new --lib crate_b
```

Cargo.toml的member中会自动添加新建的项目：

```
[workspace]
resolver = "3"
members = ["crate_a", "crate_b"]
```

3. 修改crate_b/src/lib.rs：

```rust
pub fn greet(name: &str) {
    println!("Hello, {}!", name);
}
```

4. 修改crate_a项目中的Cargo.toml：

```
[dependencies]
crate_b = { path = "../crate_b" }
```

并修改crate_a/src/main.rs：

```rust
use crate_b::greet;

fn main() {
    greet("Rust Workspace");
}
```

5. 在workspace目录运行crate_a：

```
cargo run -p crate_a
```

#### 参考资料

- [Rust Workspace Example: A Guide to Managing Multi-Crate Projects](https://medium.com/@aleksej.gudkov/rust-workspace-example-a-guide-to-managing-multi-crate-projects-82d318409260)
- [Cargo Workspaces](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)