# LLaMA.cpp Server参数优化

参考[LLaMA.cpp HTTP Server - Usage](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)

- `-m, --model`: 模型路径
- `--host`：服务地址，默认为`127.0.0.1`，设为`0.0.0.0`允许远程访问
- `--port`: 服务端口，默认为`8080`
- `--temp`：temperature设置模型的创造力，取值为0-2，默认为0.8。取值越大创造的随机性越大。0-0.2适合代码生成、数学计算、事实检查和结构化输出；0.5-0.8适合一般聊天、问答任务；1-1.5适合创造性写作、诗歌和头脑风暴。
- `-ngl, --n-gpu-layers`: GPU加载的模型层数，可以设为数字、auto或all，默认为auto，设为20表示将模型的20层加载到GPU，其他放到CPU。
- `-ncmoe, --n-cpu-moe`: 将MoE模型的前多少层留在CPU上，其他放到GPU，通常配合`-ngl 999 -ncmoe 35`使用。
- `-fa, --flash-attn`: 是否启动Flash Attention，取值为on、off或auto，默认为auto。
- `-t, --threads`: 非GPU工作使用的CPU线程数。
- `--mlock`: 将模型锁定到RAM，防止Windows swap。
- `--no-mmap`: 禁用mmap。默认情况下操作系统只在需要时才加载页面块，禁用mmap后会将整个模型提前读到内存里。
- `-ctk, --cache-type-k`: KV缓存的K类型，默认为f16，如`-ctk turbo4`
- `-ctv, --cache-type-v`: KV缓存的V类型，默认为f16，如`-ctv turbo3`
- `-np, --parallel`：并发请求的数量。
- `--api-key`: 设置api key
- `-c, --ctx-size`: 上下文长度，包括你的提示词和AI的回复，默认为0，表示从模型读取。推荐设为4096，兼顾内存用量和对话生成速度。

|Context Size|Text Length|
|--|--|
|2048|~3 pages|
|4096|~6 pages|
|8192|~12 pages|
|16384|~24 pages|

- `-n, --n-predict`: 每次回复生成token的最大长度，默认为-1，即不限制长度。1一个token约等于0.75个词。精炼快速回答推荐设为256。

|Tokens|Words|
|--|--|
|128|~100|
|256|~190|
|512|~380|
|2048|~1500|
|4096|~3000|

## 示例命令

```
# 启用embeddings
llama-server -m path/to/your_model.gguf --embeddings --pooling mean

# 6G显存优化
llama-server --model qwen3-coder-30b-a3b.gguf --n-gpu-layers 999 --n-cpu-moe 35 --no-mmap --mlock --cache-type-k turbo4 --cache-type-v turbo3
llama-server --model Qwen_Qwen3.6-35B-A3B-Q4_K_M.gguf --n-gpu-layers 999 --n-cpu-moe 35 --no-mmap --cache-type-k turbo4 --cache-type-v turbo3 --jinja -c 262144 -ub 128 -b 1024

# 8G显存运行Qwen3.8-27B
llama-server -m ~/.cache/huggingface/hub/models--unsloth--Qwen3.8-27B-GGUF/snapshots/4ca720788d1e01f1bff70c033e0d0028fd02e502/Qwen3.8-27B-UD-IQ4_XS.gguf -md ~/.cache/huggingface/hub/models--unsloth--Qwen3.8-27B-GGUF/snapshots/4ca720788d1e01f1bff70c033e0d0028fd02e502/MTP/mtp-Qwen3.8-27B-Q4_0.gguf --spec-type draft-mtp -c 70000 -ctk q4_0 -ctv q4_0 -ngl 26 -t 6
```

#### 参考资料

- [Running a 35B AI Model on 6GB VRAM, FAST (llama.cpp Guide)](https://www.youtube.com/watch?v=8F_5pdcD3HY)
- [llama-server Command Line Overview | Complete Guide to Every Option in llama.cpp](https://www.youtube.com/watch?v=N-Mc7_3z3uU)