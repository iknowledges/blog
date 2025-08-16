# PyO3调试教程

1. 新建一个pyo3demo项目，并添加pyo3依赖：

```
cargo new pyo3demo --lib
cd pyo3demo
cargo add pyo3
```

2. 修改Cargo.toml配置如下，注意lib部分是指定生成动态库：

```
[package]
name = "pyo3demo"
version = "0.1.0"
edition = "2024"

[lib]
name = "pyo3demo"
crate-type = ["cdylib"]

[dependencies]
pyo3 = "0.24.1"
```

3. 编写lib.rs如下：

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
fn pyo3demo(m: &Bound<'_, PyModule>) -> PyResult<()> {
    m.add_class::<MyClass>()?;
    Ok(())
}
```

4. 编写test.py如下：

```python
from pyo3demo import MyClass

my = MyClass(10)
print(my.num)
my.num = 20
print(my.num)
```

5. 配置.vscode/launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug PyO3 with Environment",
            "type": "lldb",
            "request": "launch",
            "program": "path/to/python.exe",
            "cwd": "${workspaceFolder}",
            "args": ["test.py"],
            "env": {
                "RUST_BACKTRACE": "1",
                "PYTHONPATH": "${workspaceFolder}/target/debug"
            },
            "sourceLanguages": ["rust"]
        }
    ]
}
```

6. 编译项目，并将生成的pyo3demo.dll文件修改为pyo3demo.pyd。

```
cargo build
```

7. 最后在lib.rs文件中打上断点就可以进行调试了。

#### 参考资料

- [PyO3 user guide](https://pyo3.rs/v0.24.1/debugging.html)
