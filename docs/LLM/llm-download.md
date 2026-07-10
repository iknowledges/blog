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

#### 其他命令

```
# 查看下载的模型缓存
hf cache ls
# 删除模型缓存
hf cache rm model/Qwen/Qwen3.6-35B-A3B
# 回收缓存垃圾占用的空间，以及中断下载后剩余的.incomplete文件
hf cache prune
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

#### 参考资料

- [Command Line Interface (CLI)](https://huggingface.co/docs/huggingface_hub/main/en/guides/cli#hf-cache)
