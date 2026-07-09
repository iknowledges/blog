# 大模型下载教程

## Huggingface

```
pip install huggingface_hub
```

- Windows中默认下载路径为`C:\Users\<用户名>\.cache\huggingface\hub`
- Linux中默认下载路径为`~/.cache/huggingface/hub`

修改默认下载路径：

```
# 设置下载路径
export HF_HOME=/path/to/huggingface
# 镜像加速
export HF_ENDPOINT=https://hf-mirror.com
```

下载时指定目录：

```
hf download deepseek-ai/DeepSeek-OCR --local_dir /path/to/huggingface
```

## ModelScope

```
pip install modelscope
```

- Windows中默认下载路径为`C:\Users\<用户名>\.cache\modelscope\hub`
- Linux中默认下载路径为`~/.cache/modelscope/hub`

修改默认下载路径：

```
export MODELSCOPE_CACHE=/path/to/modelscope
```

下载时指定目录：

```
modelscope download --model deepseek-ai/DeepSeek-OCR --local_dir /path/to/modelscope
```