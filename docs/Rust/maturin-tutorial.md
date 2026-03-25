# Maturin Tutorial

1. Create virtual environment:

```
mkdir my-project
cd my-project
uv venv
source .venv/bin/activate
uv pip install maturin
```

2. Initial project:

```
maturin init
```

3. edit `src/lib.rs`

```rust
use pyo3::prelude::*;

#[pyclass]
struct MyClass {
    a: usize,
    b: usize,
}

#[pymethods]
impl MyClass {
    #[new]
    fn new(a: usize, b: usize) -> Self {
        Self { a, b }
    }

    pub fn sum_as_string(&self) -> PyResult<String> {
        Ok((self.a + self.b).to_string())
    }
}

#[pymodule]
mod my_project {
    #[pymodule_export]
    use super::MyClass;
}
```

4. build the project

```
# build the cate and install module in current virtualenv
maturin develop
# build the wheels
maturin build
```

## Adding Python type information

1. specify a different python source directory in `pyproject.toml`

```
[tool.maturin]
python-source = "python"
```

2. add the empty `py.typed` marker file:

```
my-project
├── Cargo.toml
├── python
│   └── my_project
│       ├── __init__.py
│       ├── py.typed  # <<< add this empty file
│       └── my_project.pyi  # <<< add type stubs for Rust functions in the my_project module here
├── pyproject.toml
└── src
    └── lib.rs
```

3. edit `my_project.pyi`:

```python
class MyClass:
    def __init__(self, a: int, b: int) -> None: ...
    def sum_as_string(self) -> str: ...
```

4. test code:

```python
from my_project.my_project import MyClass

my = MyClass(1, 2)
print(my.sum_as_string())
```

#### Reference

- [Adding Python type information](https://www.maturin.rs/project_layout.html#adding-python-type-information)
