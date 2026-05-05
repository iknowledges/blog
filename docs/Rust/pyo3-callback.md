# Callback Functions Using PyO3

- src/lib.rs

```rust
use pyo3::prelude::*;

// Step 1: Define a Callback Struct in Rust
#[pyclass]
struct Callback {
    callback_function: fn() -> PyResult<()>,
}

// Step 2: Implement a Method to Call the Callback
#[pymethods]
impl Callback {
    fn __call__(&self) -> PyResult<()> {
        (self.callback_function)()
    }
}

// Step 3: Modify the Rust Fuction
#[pyfunction]
fn rust_function_1(py: Python<'_>, python_function: &Bound<'_, PyAny>) -> PyResult<()> {
    println!("This is rust_function_1");
    let callback = Bound::new(
        py,
        Callback {
            callback_function: rust_function_2,
        },
    )?;
    python_function.call1((callback,))?;
    Ok(())
}

// Step 4: Implement the Remaining Rust Code
#[pyfunction]
fn rust_function_2() -> PyResult<()> {
    println!("This is rust_function_2");
    Ok(())
}

#[pymodule]
mod rust_module {
    use super::*;

    #[pymodule_init]
    fn init(m: &Bound<'_, PyModule>) -> PyResult<()> {
        m.add_function(wrap_pyfunction!(rust_function_1, m)?)?;
        m.add_function(wrap_pyfunction!(rust_function_2, m)?)?;
        m.add_class::<Callback>()?;
        Ok(())
    }
}
```

- python_module.py

```python
import rust_module

# Step 5: Update Your Python Script
def python_function(callback):
    print("This is python_function")
    callback()

if __name__ == "__main__":
    rust_module.rust_function_1(python_function)
```

## Advance Callback Handler

```rust
use pyo3::prelude::*;

// Step 1: Define a Callback Struct in Rust
#[pyclass]
struct Callback {
    callback_function: Box<dyn Fn(Bound<'_, PyAny>) -> PyResult<()> + Send + Sync>,
}

// Step 2: Implement a Method to Call the Callback
#[pymethods]
impl Callback {
    fn __call__(&self, slf: Bound<'_, PyAny>) -> PyResult<()> {
        (self.callback_function)(slf)
    }
}

// Step 3: Modify the Rust Fuction
#[pyfunction]
fn rust_function_1(py: Python<'_>, python_function: &Bound<'_, PyAny>) -> PyResult<()> {
    println!("This is rust_function_1");
    let cb_logic: Box<dyn Fn(Bound<'_, PyAny>) -> PyResult<()> + Send + Sync> = Box::new(|_any| {
        rust_function_2()
    });
    let callback = Bound::new(
        py,
        Callback {
            callback_function: cb_logic,
        },
    )?;
    python_function.call1((callback,))?;
    Ok(())
}

// Step 4: Implement the Remaining Rust Code
#[pyfunction]
fn rust_function_2() -> PyResult<()> {
    println!("This is rust_function_2");
    Ok(())
}

#[pymodule]
mod rust_module {
    use super::*;

    #[pymodule_init]
    fn init(m: &Bound<'_, PyModule>) -> PyResult<()> {
        m.add_function(wrap_pyfunction!(rust_function_1, m)?)?;
        m.add_function(wrap_pyfunction!(rust_function_2, m)?)?;
        m.add_class::<Callback>()?;
        Ok(())
    }
}
```

#### Reference

- [How to Pass Rust Functions as Callbacks to Python Using PyO3](https://www.youtube.com/watch?v=T4904s2trPc)