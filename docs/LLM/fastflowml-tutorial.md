# FastFlowLM教程

## 安装

1. 下载[fastflowlm_0.9.45_windows_amd64.zip](https://github.com/FastFlowLM/FastFlowLM/releases)，解压后将`flx.exe`所在目录添加到系统环境变量。
2. 参照[CLI Mode](https://fastflowlm.com/docs/instructions/cli/#-changing-the-model-storage-path)，将环境变量`FLM_MODEL_PATH`设置为模型下载保存目录，如`D:\ModelCache\flm`。
3. 检查NPU驱动是否已安装：

```
flm validate
```

## 部署模型

1. 下载模型：

```
modelscope download --model FastFlowLM/Qwen3.5-9B-NPU2 --local_dir D:/ModelCache/flm/models/Qwen3.5-9B-NPU2
```

2. 启动模型服务：

```
flm serve qwen3.5:9b --host 0.0.0.0
```

3. langchain中调用服务，注意model名称要和服务一致：

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="qwen3.5:9b",
    base_url="http://127.0.0.1:52625/v1",
    api_key="flm"
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

## 其他命令

```
# 查看所有支持的模型
flm list
# 查看已下载的模型
flm list --filter installed
# 删除模型
flm remove qwen3.5:9b
```

#### 参考资料

- [RAG with LangChain + FastFlowLM + FAISS (Windows, Offline)](https://fastflowlm.com/docs/instructions/server/rag_LangChain/)