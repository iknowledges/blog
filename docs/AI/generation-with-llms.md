# 使用LLMs进行生成

LLMs（大语言模型）是文本生成背后的关键组成部分。它们包含经过大规模预训练的transformer模型，用于根据给定的输入文本预测下一个词（更准确地说是下一个token）。由于它们一次只预测一个token，因此除了调用模型之外，需要进行自回归生成来生成新的句子。

自回归生成是给定一些初始输入，通过迭代调用模型及其自身的生成输出来生成文本的推理过程。

```python
from transformers import AutoModel, AutoTokenizer


model_path = "E:\\huggingface\\chatglm3-6b"
# 加载模型，device_map确保模型被移动到GPU上
model = AutoModel.from_pretrained(model_path, trust_remote_code=True, device_map="auto")
# 加载tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)
# model_inputs变量保存着分词后的文本输入以及注意力掩码
# return_tensors：pt表示返回pytorch类型的tensor，tf表示返回TensorFlow类型的tensor
model_inputs = tokenizer(["A list of colors: red, blue"], return_tensors="pt").to("cuda")
# generate()方法生成tokens，设置max_new_tokens以控制返回的最大新tokens数量，默认为20
generated_ids = model.generate(**model_inputs, max_new_tokens=20)
# 生成的tokens转换为文本
result = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
print(result)
```

#### 参考资料

- [使用LLMs进行生成](https://hf-mirror.com/docs/transformers/main/zh/llm_tutorial)
