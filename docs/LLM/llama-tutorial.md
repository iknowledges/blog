# llama.cpp教程

## 源码安装

1. 首先确认系统中已经安装好了Nvidia驱动和CUDA：

```
nvidia-smi
nvcc -V
```

2. 下载源码

```
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
```

### Windows中编译

3. 打开PowerShell，输入下面命令开始编译：

```
mkdir build
cmake -B build -DCMAKE_BUILD_TYPE=Release -DGGML_CUDA=ON -DGGML_NATIVE=ON -DCMAKE_CUDA_ARCHITECTURES="86"
cmake --build build --config Release -j $env:NUMBER_OF_PROCESSORS
# Override directory at install time (CMake 3.15+)
cmake --install build --prefix D:/Software/llama.cpp
```

- GGML_NATIVE: ON是针对当前系统中的硬件进行编译，OFF是编译适用于所有CUDA GPU的版本。
- CMAKE_CUDA_ARCHITECTURES: 明确指定CUDA架构，参照["CUDA: Your GPU Compute > Capability"](https://developer.nvidia.com/cuda-gpus)，如`GeForce RTX 3060`的`Compute Capability`是`8.6`，那么就填`86`。

### WSL中编译

3. 打开终端，输入下面命令开始编译：

```
mkdir build
cmake -B build -DGGML_CUDA=ON -DGGML_NATIVE=ON -DCMAKE_CUDA_ARCHITECTURES="86" -DGGML_VULKAN=OFF -DGGML_CURL=ON
cmake --build build --config Release --parallel $(nproc)
cmake --install build --prefix /opt/llama.cpp
```

4. 修改`~/.bashrc`配置环境变量：

```
export LLAMA_HOME=/opt/llama.cpp
export PATH=$LLAMA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$LLAMA_HOME/lib:$LD_LIBRARY_PATH
```

- 在Ubuntu26中编译时出现如下报错：

```
CMake Error at /usr/share/cmake-4.2/Modules/CMakeDetermineCompilerId.cmake:928 (message):
  Compiling the CUDA compiler identification source file
  "CMakeCUDACompilerId.cu" failed.

......

  /usr/include/x86_64-linux-gnu/bits/mathcalls.h(206): error: exception
  specification is incompatible with that of previous function "rsqrt"
  (declared at line 629 of
  /usr/local/cuda-13.0/bin/../targets/x86_64-linux/include/crt/math_functions.h)
     extern double rsqrt (double __x) noexcept (true); extern double __rsqrt (double __x) noexcept (true);
                                      ^

  /usr/include/x86_64-linux-gnu/bits/mathcalls.h(206): error: exception
  specification is incompatible with that of previous function "rsqrtf"
  (declared at line 653 of
  /usr/local/cuda-13.0/bin/../targets/x86_64-linux/include/crt/math_functions.h)
     extern float rsqrtf (float __x) noexcept (true); extern float __rsqrtf (float __x) noexcept (true);
                                     ^
  2 errors detected in the compilation of "CMakeCUDACompilerId.cu".
```

**解决方法**：

1. 打开`math_functions.h`文件：

```
sudo vim /usr/local/cuda-13.0/targets/x86_64-linux/include/crt/math_functions.h
```

2. 搜索rsqrt关键字，在629行和653行函数声明的末尾添加`noexcept(true)`，然后保存文件重新编译：

```
// Line 629 change to:
extern __DEVICE_FUNCTIONS_DECL__ __device_builtin__ double                 rsqrt(double x) noexcept(true);

// Line 653 change to:
extern __DEVICE_FUNCTIONS_DECL__ __device_builtin__ float                  rsqrtf(float x) noexcept(true);
```

## 部署模型

1. 下载模型

```
hf download hf://unsloth/Qwen3.6-27B-GGUF/Qwen3.6-27B-UD-IQ2_M.gguf
hf download hf://unsloth/Qwen3.6-27B-GGUF/mmproj-BF16.gguf
```

2. 启动模型服务，API接口参考[LLaMA.cpp HTTP Server](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md#api-endpoints)

```
# 禁用Qwen的思考模式
export LLAMA_CHAT_TEMPLATE_KWARGS='{"enable_thinking": false}'
llama-server --model path/to/Qwen3.6-27B-UD-IQ2_M.gguf --mmproj path/to/mmproj-BF16.gguf -c 4096 --image-max-tokens 768 --host 0.0.0.0 --port 8080
# 只使用文本可以去掉mmproj
llama-server --model path/to/Qwen3.6-27B-UD-IQ2_M.gguf -c 16384 --host 0.0.0.0 --port 8080
# 启用embeddings
llama-server -m path/to/your_model.gguf --embeddings --pooling mean
# 6G显存优化
llama-server --model qwen3-coder-30b-a3b.gguf --n-gpu-layers 999 --n-cpu-moe 35 --no-mmap --mlock --cache-type-k turbo4 --cache-type-v turbo3
llama-server --model Qwen_Qwen3.6-35B-A3B-Q4_K_M.gguf --n-gpu-layers 999 --n-cpu-moe 35 --no-mmap --cache-type-k turbo4 --cache-type-v turbo3 --jinja -c 262144 -ub 128 -b 1024
```

#### 命令行参数详解

参考[LLaMA.cpp HTTP Server - Usage](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)

- `-m, --model`: 模型路径
- `--host`：服务地址，默认为`127.0.0.1`，设为`0.0.0.0`允许远程访问
- `--port`: 服务端口，默认为`8080`
- `--mmproj`: 指定支持图片、视频、音频文件的投影文件
- `--image-max-tokens`: 限制输入图像的长和宽
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
|128|~100|
|256|~190|
|512|~380|
|2048|~1500|
|4096|~3000|

## 客户端访问模型

1. 使用openai库访问服务：

```python
import openai

client = openai.OpenAI(
    base_url="http://127.0.0.1:8080/v1",
    api_key = "sk-no-key-required"
)

completion = client.completions.create(
  model="davinci-002",
  prompt="I believe the meaning of life is",
  max_tokens=8
)

print(completion.choices[0].text)
```

2. 使用langchain访问服务：

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    base_url="http://127.0.0.1:8080/v1",
    api_key="not-needed",
    model="local-model",
    temperature=0.7
)

messages = [
    (
        "system",
        "You are a helpful assistant that translates English to French. Translate the user sentence.",
    ),
    ("human", "I love programming."),
]
ai_msg = llm.invoke(messages)
print(ai_msg.text)
```

#### 参考资料

- [Build llama.cpp locally](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md)
- [How to Install Llama.cpp on Windows 11 (CUDA 13 & RTX 50-Series Guide)](https://www.youtube.com/watch?v=UALdk37JgpM)
- [How to Install Llama.cpp on Windows (WSL2) with CUDA 13.2 (Fast Tutorial)](https://www.youtube.com/watch?v=OZfxJ-rQqVE)
- [Running a 35B AI Model on 6GB VRAM, FAST (llama.cpp Guide)](https://www.youtube.com/watch?v=8F_5pdcD3HY)
- [llama-server Command Line Overview | Complete Guide to Every Option in llama.cpp](https://www.youtube.com/watch?v=N-Mc7_3z3uU)
