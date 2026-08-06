# Qwen3-TTS使用教程

## 安装

1. 创建虚拟环境：

```
uv venv --python 3.12 --seed
source .venv/bin/activate
```

2. 安装pytorch，注意要适配自己的cuda版本：

```
# CUDA 13.0
pip install torch==2.10.0 torchaudio==2.10.0 --index-url https://download.pytorch.org/whl/cu130
```

3. 下载预编译的[FlashAttention](https://github.com/Dao-AILab/flash-attention/releases)并安装（自己编译耗时太长），注意要和上面的cuda和pytorch版本一致：

```
pip install flash_attn-2.8.3+cu13torch2.10cxx11abiTRUE-cp312-cp312-linux_x86_64.whl
```

4. 安装qwen-tts：

```
pip install qwen-tts
```

也可以选择从源码安装：

```
git clone https://github.com/QwenLM/Qwen3-TTS.git
cd Qwen3-TTS
pip install -e .
```

5. 安装音频处理工具sox：

```
sudo apt update
sudo apt install -y sox libsox-fmt-all
```

## 测试

1. 编写如下测试代码：

```python
import torch
import soundfile as sf
from qwen_tts import Qwen3TTSModel

model = Qwen3TTSModel.from_pretrained(
    "Qwen/Qwen3-TTS-12Hz-1.7B-Base",
    device_map="cuda:0",
    dtype=torch.bfloat16,
    attn_implementation="flash_attention_2",
)

ref_audio = "https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen3-TTS-Repo/clone.wav"
ref_text  = "Okay. Yeah. I resent you. I love you. I respect you. But you know what? You blew it! And thanks to you."

wavs, sr = model.generate_voice_clone(
    text="I am solving the equation: x = [-b ± √(b²-4ac)] / 2a? Nobody can — it's a disaster (◍•͈⌔•͈◍), very sad!",
    language="English",
    ref_audio=ref_audio,
    ref_text=ref_text,
)
sf.write("output_voice_clone.wav", wavs[0], sr)
```

2. 网页端使用：

```
qwen-tts-demo Qwen/Qwen3-TTS-12Hz-1.7B-Base --ip 0.0.0.0 --port 8000
```

- 解决如下报错：

```
TypeError: GZipResponder.__init__() missing 1 required keyword-only argument: 'thread_minimum_size'
```

根据Starlette的[Release notes](https://starlette.dev/release-notes/)中1.4.1版本已解决这个bug：

```
pip install "starlette>=1.4.1"
```

#### 参考资料

- [Qwen/Qwen3-TTS-12Hz-1.7B-Base](https://huggingface.co/Qwen/Qwen3-TTS-12Hz-1.7B-Base)
