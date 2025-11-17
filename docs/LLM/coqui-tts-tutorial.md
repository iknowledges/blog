# Coqui TTS使用教程

1. 安装TTS

```
git clone https://github.com/coqui-ai/TTS
pip install -e .[dev]
```

这里直接安装在使用下载好的模型时会报错，建议参考后面的报错解决修改源码再安装。

2. 手动下载模型，打开源代码目录下的TTS/.models.json文件，如下载模型"tts_models/en/ljspeech/tacotron2-DDC"，就可以找到如下链接进行下载：

```json
"tts_models": {
    "en": {
        "ljspeech": {
            "tacotron2-DDC": {
                "description": "Tacotron2 with Double Decoder Consistency.",
                "github_rls_url": "https://coqui.gateway.scarf.sh/v0.6.1_models/tts_models--en--ljspeech--tacotron2-DDC.zip",
                "default_vocoder": "vocoder_models/en/ljspeech/hifigan_v2",
                "commit": "bae2ad0f",
                "author": "Eren Gölge @erogol",
                "license": "apache 2.0",
                "contact": "egolge@coqui.com"
            },
        }
    }
}
```

3. 测试代码

```python
from TTS.api import TTS

tts = TTS(model_path="./tts_models--en--ljspeech--tacotron2-DDC/model_file.pth", config_path="./tts_models--en--ljspeech--tacotron2-DDC/config.json")
tts.tts_to_file(text="Hello World!", file_path="output.wav")
```

### 报错解决

- AttributeError: 'TTS' object has no attribute 'is_multi_lingual'

解决方法：找到源码目录下的TTS/api.py，将第102行由：

```python
# Not sure what sets this to None, but applied a fix to prevent crashing.
if (
    isinstance(self.model_name, str)
    and "xtts" in self.model_name
    or self.config
    and ("xtts" in self.config.model or len(self.config.languages) > 1)
):
```

修改为：

```python
# Not sure what sets this to None, but applied a fix to prevent crashing.
if (
    isinstance(self.model_name, str)
    and "xtts" in self.model_name
    or self.config
    and ("xtts" in self.config.model or "language" in self.config and len(self.config.languages) > 1)
):
```

#### 参考资料

- [Coqui TTS](https://github.com/coqui-ai/TTS)
