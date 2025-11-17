# MinerU部署教程

## 安装

1. 创建虚拟环境

```
python -m venv mineru
source mineru/bin/activate
```

2. 源码安装MinerU

```
git clone https://github.com/opendatalab/MinerU.git
cd MinerU
pip install -e .[all]
```

3. 执行模型下载命令：

```
mineru-models-download
```

选择modelscope和all

```
Please select the model download source:  (huggingface, modelscope) [huggingface]: modelscope
Please select the model type to download:  (pipeline, vlm, all) [all]: all
```

模型下载完成后会在家目录生成配置文件`~/mineru.json`，检查model-dir下的pipline和vlm设置的模型路径是否正确。

4. 启动服务

```
# 将模型源设为本地
export MINERU_MODEL_SOURCE=local
# 如果有多个GPU需要指定使用哪一个，可以使用nvidia-smi查看GPU序号
export CUDA_VISIBLE_DEVICES=0
# 查看接口文档: http://127.0.0.1:50000/docs
mineru-api --host 0.0.0.0 --port 50000
```

## 测试代码

```python
import requests
from pathlib import Path

def test_mineru_api(pdf_path: str, backend: str = "pipeline"):
    api_url = "http://127.0.0.1:50000/file_parse"
    print(f"\n{'='*60}")
    print(f"测试 MinerU API with backend: {backend}")
    print(f"{'='*60}")
    print(f"API URL: {api_url}")
    print(f"PDF 文件: {pdf_path}")

    if not Path(pdf_path).exists():
        print(f"文件不存在: {pdf_path}")
        return None
    
    try:
        with open(pdf_path, 'rb') as f:
            files = [('files', (Path(pdf_path).name, f, 'application/pdf'))]
            data = {
                'backend': backend,
                'parse_method': 'auto',
                'return_md': 'true',
                'return_middle_json': 'false',
                'return_model_output': 'false',
                'return_content_list': 'false',
                'start_page_id': '0',
                'end_page_id': '1'
            }
            print('发送请求...')
            response = requests.post(
                api_url,
                files=files,
                data=data,
                timeout=3000
            )
        
        if response.status_code != 200:
            print(f"请求失败: HTTP {response.status_code}")
            print(f"响应: {response.text[:500]}")
            return None
        result = response.json()
        backend_used = result.get('backend', 'unknown')
        version = result.get('version', 'unknown')
        results = result.get('results', {})
        print("请求成功!")
        print(f"使用backend: {backend_used}")
        print(f"版本: {version}")
        print(f"结果数量: {len(results)}")

        if results:
            file_key = list(results.keys())[0]
            md_content = results[file_key].get('md_content', '')
            
            output_file = f"output_{backend}.md"
            with open(output_file, 'w', encoding='utf-8') as f:
                f.write(md_content)
            print(f"\n完整 Markdown 已保存到: {output_file}")
            return md_content
        else:
            print("未找到结果")
            return None
    except Exception as e:
        print(f"测试失败: {e}")
        import traceback
        traceback.print_exc()
        return None
    
def main():
    pdf_path = "test.pdf"
    result_pipline = test_mineru_api(pdf_path, backend="pipeline")
    if result_pipline:
        print(f"\npipline结果预览: {result_pipline[:500]}")
    result_vllm = test_mineru_api(pdf_path, backend="vlm-vllm-async-engine")
    if result_vllm:
        print(f"\nvllm结果预览: {result_vllm[:500]}")


if __name__ == '__main__':
    main()
```
