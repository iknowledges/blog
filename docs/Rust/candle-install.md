# Candle安装使用教程

1. 首先测试一下Cuda是否已经安装：

```
nvcc --version
nvidia-smi --query-gpu=compute_cap --format=csv
```

2. 新建一个Rust项目

```
cargo new myapp
cd myapp
```

3. 添加candle-core库

```
# GPU版本
cargo add --git https://github.com/huggingface/candle.git candle-core --features "cuda"
# CPU版本
cargo add --git https://github.com/huggingface/candle.git candle-core
```

4. 编写main.rs

```rust
use candle_core::{Result, Device, Tensor};

fn main() -> Result<()> {
    let device = Device::new_cuda(0)?;

    let a = Tensor::randn(0f32, 1., (2, 3), &device)?;
    let b = Tensor::randn(0f32, 1., (3, 4), &device)?;
    
    let c = a.matmul(&b)?;
    println!("{c}");
    Ok(())
}
```

#### 参考资料

- [candle](https://github.com/huggingface/candle)
- [Candle Documentation](https://huggingface.github.io/candle/guide/installation.html)