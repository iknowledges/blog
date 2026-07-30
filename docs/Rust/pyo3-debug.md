# PyO3调试教程

1. 首先安装必需的VSCode插件：

- [Rust Analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)
- [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

2. 新建一个pyo3demo项目，并添加pyo3依赖：

```
cargo new pyo3demo --lib
cd pyo3demo
cargo add pyo3
```

目录结构如下：

```
pyo3demo/
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    └── my_test.py
```

3. 修改Cargo.toml配置如下，注意lib部分是指定生成动态库：

```
[package]
name = "pyo3demo"
version = "0.1.0"
edition = "2024"

[lib]
name = "pyo3lib"
crate-type = ["cdylib"]

[dependencies]
pyo3 = "0.29.0"
```

4. 编写`src/lib.rs`如下：

```rust
use pyo3::prelude::*;

#[pyclass]
struct MyClass {
    num: i32,
}

#[pymethods]
impl MyClass {
    #[new]
    fn new(num: i32) -> Self {
        MyClass { num }
    }
    #[getter]
    fn get_num(&self) -> PyResult<i32> {
        Ok(self.num)
    }
    #[setter]
    fn set_num(&mut self, num: i32) -> PyResult<()> {
        self.num = num;
        Ok(())
    }
}

#[pymodule]
mod pyo3lib {
    #[pymodule_export]
    use super::MyClass;
}
```

5. 创建好虚拟环境，并安装pytest：

```
pip install pytest
```

6. 编写测试代码`tests/my_test.py`如下：

```python
import pytest
from pyo3lib import MyClass

def test_function():
    my = MyClass(10)
    assert my.num == 10
    my.num = 20
    assert my.num == 20
```

7. 创建`.vscode/launch.json`配置如下：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug PyO3 with Environment",
            "type": "lldb",
            "request": "launch",
            "program": "${workspaceFolder}/.venv/bin/python",
            "args": ["-m", "pytest", "tests/my_test.py::test_function", "-v"],
            "cwd": "${workspaceFolder}",
            "env": {
                "RUST_BACKTRACE": "1",
                "PYTHONPATH": "${workspaceFolder}/target/debug"
            },
            "sourceLanguages": ["rust"]
        }
    ]
}
```

6. 编译项目，并修改生成的文件名，不改就会找不到相应模块。Linux环境将libpyo3lib.so修改为pyo3lib.so，Windows环境将pyo3demo.dll修改为pyo3demo.pyd。

```
cargo build
```

7. 最后在lib.rs文件中打上断点就可以进行调试了。

#### 参考资料

- [PyO3 user guide](https://pyo3.rs/v0.29.0/debugging.html)
