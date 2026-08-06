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
```

- `--mmproj`: 指定支持图片、视频、音频文件的投影文件
- `-c`: 上下文长度
- `--image-max-tokens`: 限制输入图像的长和宽
- `--host 0.0.0.0`：允许远程访问

#### 其他指令

```
# 启用embeddings
llama-server -m path/to/your_model.gguf --embeddings --pooling mean
```

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

- [How to Install Llama.cpp on Windows 11 (CUDA 13 & RTX 50-Series Guide)](https://www.youtube.com/watch?v=UALdk37JgpM)
- [How to Install Llama.cpp on Windows (WSL2) with CUDA 13.2 (Fast Tutorial)](https://www.youtube.com/watch?v=OZfxJ-rQqVE)
- [Build llama.cpp locally](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md)
