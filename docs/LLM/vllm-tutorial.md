# vLLM教程

## 安装GPU版本

1. 创建虚拟环境，注意当前支持的Python版本为`3.10 - 3.13`：

```
uv venv llm --python 3.12 --seed --managed-python
source llm/bin/activate
```

2. 安装vllm

```
uv pip install vllm --torch-backend=auto
```

3. 安装成功后查看版本号

```
vllm --version
```

## 部署模型

```
vllm serve Qwen/Qwen3.5-2B --port 8000 --tensor-parallel-size 1 --max-model-len 262144 --reasoning-parser qwen3 --gpu-memory-utilization 0.85
```

--gpu-memory-utilization: 默认为0.9，要根据自己电脑配置调整合适的值，以解决如下报错：

```
RuntimeError: Engine core initialization failed. See root cause above. Failed core proc(s): {}
```

vllm参数调整参考：

```
--gpu-memory-utilization 0.6-0.7
--max-model-len 2048-4096
--enforce-eager  # Avoids CUDA graph overhead
--dtype bfloat16  # If your GPU supports it
```

#### 参考资料

- [Engine Core initialization failed. See root cause above](https://github.com/vllm-project/vllm/issues/17618)
- [Run vLLM Locally on Low-VRAM Budget Laptop (4GB GPU) in 2025: Full Docker Guide, Errors & Ollama Alternatives and Ultimate Success](https://kumarshivam-66534.medium.com/run-vllm-locally-on-low-vram-budget-laptop-4gb-gpu-in-2025-full-docker-guide-errors-ollama-bf8c498e7dec)
