# Rust单例模式

- 使用 lazy_static!

```rust
#[macro_use]
extern crate lazy_static;

use std::sync::Mutex;

struct MySingleton {
    data: String,
}

lazy_static! {
    static ref INSTANCE: Mutex<MySingleton> = Mutex::new(MySingleton {
        data: "Initial data".to_string(),
    });
}

impl MySingleton {
    pub fn get_instance() -> &'static Mutex<MySingleton> {
        &*INSTANCE
    }
}
```

- 使用 once_cell

```rust
use once_cell::sync::OnceCell;
use std::sync::Mutex;

struct MySingleton {
    data: String,
}

static INSTANCE: OnceCell<Mutex<MySingleton>> = OnceCell::new();

impl MySingleton {
    pub fn get_instance() -> &'static Mutex<MySingleton> {
        INSTANCE.get_or_init(|| {
            Mutex::new(MySingleton {
                data: "Initial data".to_string(),
            })
        })
    }
}
```

- 使用 static mut 和 unsafe

```rust
use std::sync::Mutex;

struct MySingleton {
    data: String,
}

static mut INSTANCE: Option<Mutex<MySingleton>> = None;

impl MySingleton {
    pub fn get_instance() -> &'static Mutex<MySingleton> {
        unsafe {
            if INSTANCE.is_none() {
                INSTANCE = Some(Mutex::new(MySingleton {
                    data: "Initial data".to_string(),
                }));
            }
            INSTANCE.as_ref().unwrap()
        }
    }
}
```

#### 参考资料

- [Singleton in Rust](https://refactoring.guru/design-patterns/singleton/rust/example#example-0)