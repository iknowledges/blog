# BERT从零详细解读

## 1 BERT整体模型架构

Bert的基础架构用的是Transformer的Encoder部分，然后将多个基础架构堆叠在一起，其中Bert base是12层Encoder，Bert large是24层Encoder。

Bert的输入部分和Transformer有所不同，由三部分组成：token emb、segment emb和position emb。

![bert01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/BERT/bert01.png)

- Input包括分词器产生的符号、特殊符号和正常单词。其中##ing是bert分词器产生的，不需要关注。

特殊符号用于bert预训练中的NSP任务：
[SEP]用于分割句子，表示符号之前是一个句子，符号之后是另一个句子。
[CLS]的输出向量接一个二分类器去做二分类任务。

注意：CLS向量不能代表语义信息。bert pretrain模型直接拿来用作sentence embedding效果甚至不如word embedding，cls的emebdding效果最差。

- Token Embeddings：对Input中所有的词汇做正常的Embedding，比如随机初始化。
- Segment Embeddings: 区分不同的句子，比如第一个句子全部用0表示，第二个句子全部用1表示。
- Position Embeddings: 与Transformer用正余弦函数不同，bert中使用的是随机初始化，然后让模型自己去学习出来。比如第一个位置是0，第二个是1，依次类推，一直到511。

## 2 如何做BERT预训练：MLM+NSP

## 2.1 MLM(Mask Language Model)

### 2.1.1 无监督目标函数有两种：AR和AE

1. AR(autoregressive)，自回归模型：只能考虑单侧的信息，典型的就是GPT
2. AE(autoencoding)，自编码模型：从损坏的输入数据中预测重建原始数据。可以使用上下文的信息

### 2.1.2 示例任务：【我爱吃饭】

- AR的优化目标是：P(我爱吃饭) = P(我)P(爱|我)P(吃|我爱)P(饭|我爱吃)

- AE是进行mask之后：【我爱mask饭】，优化目标变成：P(我爱吃饭|我爱mask饭)=P(吃|我爱饭)

MLM预测的是mask这个单词是什么意思，本质是打破了文本，让文本重建。

### 2.1.3 mask模型的缺点：

假如mask两个单词之后：【我爱mask mask】，优化目标变成：P(我爱吃饭|我爱mask mask)=P(吃|我爱)P(饭|我爱)

这时模型认为“吃”和“饭”是相互独立的，也就是mask和mask是相互独立的，但实际上他们之前是有关系的，两个mask并不独立。

### 2.1.4 mask概率问题

bert随机mask15%的单词，这其中的10%替换成其他，10%保持不变，80%替换为mask。

mask代码：

```python
# mask_indices就是15%的单词
for index in mask_indices:
    #80% of the time, replace with [MASK] if random.random()<0.8:
    masked_token = "[MASK]" else:
    #10% of the time,keep original if random.random()<0.5:
    masked_token =tokens[index]
    # 10% of the time, replace with random word else:
    masked_token = random.choice(vocab_list) 
```

## 2.2 NSP(Next Sentence Prediction)

NSP用于判断两个句子之间的关系。是一个二分类任务。

NSP样本的构造模式如下:

1. 从训练语料库中取出两个连续的段落作为正样本（两个连续的段落说明主题一致，并且有顺序）
2. 从不同的文档中随机创建一对段落作为负样本（不同文档说明主题不一致，并且没有顺序）

缺点：主题预测和连贯性预测合并为一个单项任务，主题预测相对于连贯性预测更为简单，从而间接简化了任务

## 3 如何微调BERT，提升BERT在下游任务中的效果

![bert02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/BERT/bert02.png)

### 3.1 微调Bert有四种：

- (a)文本匹配的任务：就是把两个句子拼接起来，去判断他们是否相似，也就是用CLS做二分类
- (b)单个句子的分类任务：将CLS的输出微调，做二分类或者多分类任务
- (c)问答任务
- (d)序列标注任务：把所有的Token输出，做softmax，然后去看属于实体中的哪一个。

### 3.2 如何提升BERT在下游任务的表现

#### 3.2.1 一般步骤

1. 获取谷歌中文BERT
2. 基于任务数据进行微调

#### 3.2.2 示例步骤

例如做微博文本情感分析可以分为四步骤走：

1. 在大量通用语料上训练一个Languiage Model(Pretrain)--中文谷歌BERT
2. 在相同领域上继续训练Languiage Model(Domain transfer)--在大量微博文本上继续训练这个BERT（有的文本不属于情感分析的范畴）
3. 在任务相关的小数据上继续训练Languiage Model(Task transfer)--在微博情感文本上训练
4. 在任务相关数据上做具体任务（Fine-tune）。

一般经验：先做Domain transfer，再进行Task transfer，最后Fine-tune性能是最好的。

#### 3.2.3 如何在相同领域数据中进行further pre-training

1. 动态mask：就是每次epoch去训练的时候mask，而不是一直使用同一个。
2. n-gram mask：其实比如ERNIE和SpanBert都是类似于做了实体词的mask 

参数设置经验：

- Batch size：16,32影响不太大
- Learning rate(Adam)：5e-5,3e-5,2e-5，尽可能小一点避免灾难性遗忘 
- Number of epochs：一般3,4
- Weighted decay修改后的adam，使用warmup，搭配线性衰减 

数据增强/自蒸馏/外部知识的融入

### 参考资源

- [BERT从零详细解读](https://www.bilibili.com/video/BV1Ey4y1874y)
