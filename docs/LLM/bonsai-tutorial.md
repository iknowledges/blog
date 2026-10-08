# Bonsai模型llama.cpp部署教程

1. 原版llama.cpp不能识别PQ2_0和PTQ1_0类型，所需要安装[PrismML-Eng/llama.cpp](https://github.com/PrismML-Eng/llama.cpp)

```
git clone https://github.com/PrismML-Eng/llama.cpp && cd llama.cpp
# drop -DGGML_CUDA=ON on macOS, Metal is default
cmake -B build -DGGML_CUDA=ON && cmake --build build -j
```

2. 下载模型

```
hf download hf://prism-ml/Ternary-Bonsai-2-27B-gguf/Ternary-Bonsai-2-27B-PQ2_0.gguf
```

3. 启动模型

```
./build/bin/llama-server -m ~/.cache/huggingface/hub/models--prism-ml--Ternary-Bonsai-2-27B-gguf/snapshots/b072e1d3b35a0a630cece372c2127528e0994386/Ternary-Bonsai-2-27B-PQ2_0.gguf -ngl 99 -fa on -c 32768 --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.05 -n 16384 --host 0.0.0.0 --port 9931
```

#### 参考资料

- [prism-ml/Ternary-Bonsai-2-27B-gguf](https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-gguf)