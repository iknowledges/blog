# DeepSeek-OCR部署教程

## 一、模型下载

模型地址：[DeepSeek-OCR](https://modelscope.cn/models/deepseek-ai/DeepSeek-OCR)

```
modelscope download --model deepseek-ai/DeepSeek-OCR --local_dir /path/to/modelscope
```

## 二、官方项目安装

1. 克隆项目：

```
git clone https://github.com/deepseek-ai/DeepSeek-OCR.git
```

2. 执行安装命令，需要手动下载[vllm-0.8.5](https://github.com/vllm-project/vllm/releases/download/v0.8.5/vllm-0.8.5+cu118-cp38-abi3-manylinux1_x86_64.whl)和[flash_attn-2.7.3](https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu11torch2.6cxx11abiFALSE-cp312-cp312-linux_x86_64.whl)：

```
# 创建虚拟环境
python3 -m venv deepseek-ocr
source deepseek-ocr/bin/activate
# 安装依赖
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu118
pip install vllm-0.8.5+cu118-cp38-abi3-manylinux1_x86_64.whl
pip install -r requirements.txt
# 这个命令安装卡死：pip install flash-attn==2.7.3 --no-build-isolation
pip install flash_attn-2.7.3+cu11torch2.6cxx11abiFALSE-cp312-cp312-linux_x86_64.whl
```

注意忽略如下错误：

```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
vllm 0.8.5+cu118 requires tokenizers>=0.21.1, but you have tokenizers 0.20.3 which is incompatible.
vllm 0.8.5+cu118 requires transformers>=4.51.1, but you have transformers 4.46.3 which is incompatible.
```

3. 

## 三、官方案例

示例代码参考：https://modelscope.cn/models/deepseek-ai/DeepSeek-OCR

- transformers

```python
from modelscope import AutoModel, AutoTokenizer
import torch
import os
os.environ["CUDA_VISIBLE_DEVICES"] = '0'
model_name = 'deepseek-ai/DeepSeek-OCR'

tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=True)
model = AutoModel.from_pretrained(model_name, _attn_implementation='flash_attention_2', trust_remote_code=True, use_safetensors=True)
model = model.eval().cuda().to(torch.bfloat16)

# prompt = "<image>\nFree OCR. "
prompt = "<image>\n<|grounding|>Convert the document to markdown. "
image_file = './input/test.png'
output_path = './output/'

res = model.infer(tokenizer, prompt=prompt, image_file=image_file, output_path = output_path, base_size = 1024, image_size = 640, crop_mode=True, save_results = True, test_compress = True)
```

- vllm

安装新版的[vllm](https://docs.vllm.ai/projects/recipes/en/latest/DeepSeek/DeepSeek-OCR.html#installing-vllm)，注意这里会自动卸载掉上面的vllm-0.8.5，所以最好另外创建一个虚拟环境。

```
pip install -U vllm --pre --extra-index-url https://wheels.vllm.ai/nightly --extra-index-url https://download.pytorch.org/whl/cu118
```

```python
from vllm import LLM, SamplingParams
from vllm.model_executor.models.deepseek_ocr import NGramPerReqLogitsProcessor
from PIL import Image

# Create model instance
llm = LLM(
    model="deepseek-ai/DeepSeek-OCR",
    enable_prefix_caching=False,
    mm_processor_cache_gb=0,
    logits_processors=[NGramPerReqLogitsProcessor]
)

# Prepare batched input with your image file
image_1 = Image.open("./input/test1.png").convert("RGB")
image_2 = Image.open("./input/test2.png").convert("RGB")
prompt = "<image>\nFree OCR."

model_input = [
    {
        "prompt": prompt,
        "multi_modal_data": {"image": image_1}
    },
    {
        "prompt": prompt,
        "multi_modal_data": {"image": image_2}
    }
]

sampling_param = SamplingParams(
            temperature=0.0,
            max_tokens=8192,
            # ngram logit processor args
            extra_args=dict(
                ngram_size=30,
                window_size=90,
                whitelist_token_ids={128821, 128822},  # whitelist: <td>, </td>
            ),
            skip_special_tokens=False,
        )
# Generate output
model_outputs = llm.generate(model_input, sampling_param)

# Print output
for output in model_outputs:
    print(output.outputs[0].text)
```
