# PyO3中Rust调用Python

1. 创建一个Rust项目，`Cargo.toml`中添加如下依赖：

```
[dependencies]
pyo3 = { version = "0.28.3", features = ["auto-initialize"] }
```

2. 在项目根目录创建`pysrc/my_test.py`：

```python
class Calculator:
    def __init__(self, a):
        self.a = a
    def add(self, b):
        return self.a + b
```

3. 修改`src/main.rs`：

```rust
use std::path::Path;
use pyo3::{prelude::*, types::PyList};

fn main() -> PyResult<()> {
    let path = Path::new("./pysrc");
    Python::attach(|py| {
        let syspath = py.import("sys")?
            .getattr("path")?
            .cast_into::<PyList>()?;
        syspath.insert(0, path.to_str())?;
        println!("{:?}", syspath);

        let my_module = PyModule::import(py, "my_test")?;
        let calculator_class = my_module.getattr("Calculator")?;
        let calculator = calculator_class.call1((1,))?;
        let result = calculator.call_method1("add", (2,))?;
        let num: i32 = result.extract()?;
        println!("{}", num);
        Ok(())
    })
}
```