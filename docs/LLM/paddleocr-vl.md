# PaddleOCR-VL部署教程

## 安装PaddlePaddle和PaddleOCR

1. 创建虚拟环境

```
python -m venv paddleocr
source paddleocr/bin/activate
```

2. 安装[PaddlePaddle](https://www.paddlepaddle.org.cn/)

```
pip install paddlepaddle-gpu==3.2.2 -i https://www.paddlepaddle.org.cn/packages/stable/cu126/
```

检查是否安装成功

```python
import paddle
print(paddle.utils.run_check())
```

3. 安装PaddleOCR

```
pip install "paddleocr[doc-parser]"
```

4. 安装safetensors，PaddleOCR-VL要使用它存储模型权重

```
# 对于 Linux 系统，执行：
pip install https://paddle-whl.bj.bcebos.com/nightly/cu126/safetensors/safetensors-0.6.2.dev0-cp38-abi3-linux_x86_64.whl
# 对于Windows 系统，执行：
pip install https://xly-devops.cdn.bcebos.com/safetensors-nightly/safetensors-0.6.2.dev0-cp38-abi3-win_amd64.whl
```

## 下载PaddleOCR-VL模型

```
modelscope download --model PaddlePaddle/PaddleOCR-VL
```

## 启动VLM推理服务

1. 安装推理加速服务依赖，`paddleocr install_genai_server_deps`命令在执行过程中需要使用CUDA编译工具，需要安装[flash_attn-2.8.2](https://github.com/mjun0812/flash-attention-prebuild-wheels/releases/download/v0.3.14/flash_attn-2.8.2+cu128torch2.8-cp312-cp312-linux_x86_64.whl)

```
pip install flash_attn-2.8.2+cu128torch2.8-cp312-cp312-linux_x86_64.whl
paddleocr install_genai_server_deps vllm
```

2. 启动vLLM服务

```
paddlex_genai_server --model_name PaddleOCR-VL-0.9B --backend vllm --host 0.0.0.0 --port 8118
```

## 手动安装依赖部署

1. 执行以下命令，通过PaddleX CLI安装服务化部署插件：

```
paddlex --install serving
```

2. 然后生成PaddleOCR-VL.yaml配置文件

```
paddlex --get_pipeline_config PaddleOCR-VL
```

修改配置文件：

```yaml
VLRecognition:
  ...
  genai_config:
    backend: vllm-server
    server_url: http://127.0.0.1:8118/v1
```

3. 启动PaddleOCR API服务:

```
paddlex --serve --pipeline PaddleOCR-VL.yaml --port 10800 --host 0.0.0.0 --paddle_model_dir /path/to/PaddlePaddle
```

成功后可以访问文档地址：http://127.0.0.1:10800/docs

## 测试代码

```python
import json
import requests
import base64

SERVER_URL = "http://127.0.0.1:10800/layout-parsing"
input_path = "./test.pdf"
output_md = "result.md"

with open(input_path, "rb") as f:
    file_base64 = base64.b64encode(f.read()).decode("utf-8")
    payload = {
        "file": file_base64,
        "fileType": 0 if input_path.lower().endswith(".pdf") else 1,
        "prettifyMarkdown": True,
        "visualize": False
    }

    headers = {"Content-Type": "application/json"}
    resp = requests.post(SERVER_URL, headers=headers, data=json.dumps(payload))
    if resp.status_code == 200:
        data = resp.json()
        if data.get("errorCode") == 0:
            results = data["result"]["layoutParsingResult"]
            md_text = ""
            for i, page in enumerate(results, 1):
                md_text += f"\n\n# Page {i}\n\n"
                md_text += page["markdown"]["text"]
            
            with open(output_md, "w", encoding="utf-8") as f:
                f.write(md_text)
            print(f"成功生成 Markdown: {output_md}")
        else:
            print(f"服务器错误: {data.get('errorMsg')}")
    else:
        print(f"HTTP 错误: {resp.status_code}")
        print(resp.text)
```

#### 参考资料

- [PaddleOCR-VL 使用教程](https://www.paddleocr.ai/latest/version3.x/pipeline_usage/PaddleOCR-VL.html)