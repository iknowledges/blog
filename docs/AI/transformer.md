# Transformer模型

# 一、TRM在做一个什么事情

![TRM01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/TRM01.png)

![TRM02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/TRM02.png)

![TRM03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/TRM03.png)

# 二、Transformer结构

![transformer01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer01.png)

## Encoder

![transformer02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer02.png)

### 1 输入部分

#### 1.1 Embedding

![transformer03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer03.png)

Embedding每一行是一个512维的词向量，这个句子总共包括12个词。

#### 1.2 位置嵌入

需要位置编码的原因：

![transformer04](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer04.png)

位置编码公式：

$$
\begin{aligned}
PE_{(pos,2i)}& =sin(pos/10000^{2i/d_{\mathrm{model}}})  \\
PE_{(pos,2i+1)}& =cos(pos/10000^{2i/d_{\mathrm{model}}})  \\
\end{aligned}
$$

![transformer05](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer05.png)

如图公式中pos代表词的位置，512维中的偶数下标使用正弦，奇数下标使用余弦。

![transformer06](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/transformer06.png)

然后将每个单词的Embedding和位置编码直接相加。

- 引申一下为什么位置嵌入会有用

借助上述公式，我们可以得到一个特定位置的$d_{model}$维的位置向量，并且借助三角函数的性质：

$$
\left.\left\{\begin{matrix}
sin(\alpha+\beta)=sin\alpha cos\beta+cos\alpha sin\beta\\
cos(\alpha+\beta)=cos\alpha cos\beta-sin\alpha sin\beta
\end{matrix}\right.\right.
$$

我们可以得到：

$$
\left.\left\{\begin{matrix}
PE(pos+k,2i)=PE(pos,2i)\times PE(k,2i+1)+PE(pos,2i+1)\times PE(k,2i)\\
PE(pos+k,2i+1)=PE(pos,2i+1)\times PE(k,2i+1)-PE(pos,2i)\times PE(k,2i)
\end{matrix}\right.\right.
$$

可以看出，对于pos＋k位置的位置向量某一维2i或2i＋1而言，可以表示为，pos位置与k位置的位置向量的2i与2i＋1维的线性组合，这样的线性组合意味着位置向量中蕴含了相对位置信息。
但是这种相对位置信息会在注意力机制那里消失。

### 2 注意力机制

#### 2.1 基本的注意力机制

公式：

$$
\text{Attention}(Q,K,V)=\text{softmax}(\frac{QK^T}{\sqrt{d_k}})V
$$

![attention02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention02.png)

1. F(Q,K)表示计算两个向量相似度的函数，比如将向量Query和向量Key1做点乘，得到一个相似度的标量s1。
2. 然后进行SoftMax归一化使得a1、a2、a3、a4的总和为1。
3. 最后对Value矩阵做加权和，得到Attention Value矩阵。

点乘的结果是一个向量在另一个向量投影的长度，它可以反应两个向量的相似度。

#### 2.2 在TRM中的注意力

1. 在只有单词向量的情况下，获取QKV

![attention03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention03.png)

注意单词向量$X_1,X_2$使用的同一组权重参数$W^Q,W^K,W^V$。

2. 计算QK相似度，得到attention值

![attention04](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention04.png)

除以$\sqrt{d_k}$的原因：q和k相乘后值很大，Softmax后得到的梯度很小，从而就会引起梯度消失，除以$\sqrt{d_k}$就可以保证方差控制为1。

3. 实际代码中使用矩阵，方便并行计算

![attention05](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention05.png)

4. 多头注意力机制

![attention06](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention06.png)

上面计算过程中只使用了一组参数$W^Q,W^K,W^V$，而多头注意力机制中会用到多组参数。

可以理解为不同的头代表不同的空间，这样Transformer就可以捕获到不同空间的特征信息。

![attention07](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/attention07.png)

多个头就会有多个输出，需要合在一起进行输出。

#### 2.3 残差和LayNorm

![residual_laynorm](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/residual_laynorm.png)

由下往上，x1、x2代表词向量，往上的加号表示和位置编码进行对位相加，得到新的x1、x2。

残差是指将自注意力层的输出Z与原来输入的X进行相加，即图中的X+Z。

#### 2.3.1 残差

![residual01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/residual01.png)

假设所有weight layer组成的神经网络的输出是一个函数F(x)，那么将F(x)加上原来的输入x就可以得到一个新的输出，即F(x)+x，也就是残差。

残差的作用：

![residual02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/residual02.png)

A, B, C, D为四个不同的网络块，箭头代表数据流。

网络A的输出就是上一个图的x，D的输入就是F(x)+x。

根据反向传播的链式法则：

$$
\begin{aligned}
\frac{\partial L}{\partial X_{Aout}}&=\frac{\partial L}{\partial X_{Din}}\frac{\partial X_{Din}}{\partial X_{Aout}}\\
X_{Din}&=X_{Aout}+C(B(X_{Aout}))
\end{aligned}
$$

所以：

$$
\frac{\partial L}{\partial X_{Aout}}=\frac{\partial L}{\partial X_{Din}}(1+\frac{\partial X_{C}}{\partial X_{B}}\frac{\partial X_{B}}{\partial X_{Aout}})
$$

因为梯度消失一般情况下是连乘产生的，括号中的1确保了梯度不会为0，从而缓解了梯度消失的出现了。这也是使用了残差的神经网络可以变得比较深的原因。

#### 2.3.2 Layer Normalization和Batch Normalization

BN(Batch Normalization)在NLP中效果差，所以用的少。

- BN的优点：

1. 可以解决内部协变量偏移
2. 缓解了梯度饱和问题（如果使用sigmoid激活函数），加快收敛。

![layer_norm_01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/layer_norm_01.png)

BN是针对整个batch中的样本在同一维度的特征做处理。如图中$x^1,…x^R$代表不同的样本，每一行代表同一个特征。

- BN的缺点：

1. batch_size较小的时候，效果差。
2. BN在RNN中效果比较差。

![layer_norm_02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/layer_norm_02.png)

BN中的一个假设是使用batch中的均值和方差来模拟全部数据的均值和方差。

如图batch size为10，其中9个样本的句子长度是5个单词，1个样本的句子长度是20个单词，前5个单词的均值和方差可以用10个样本算出来，但是第6到20个单词只有一个样本，所以就回到BN第一个缺点，batch size太小算出来的效果很差，这就是BN在RNN中效果不好的原因。

- 使用LayerNorm的原因：LN的特点是对同一个样本中的所有单词做缩放。

例如上一个图中最后一个句子有20个词，那么LN就是对这20个单词做缩放，求这20个词的均值和方差。

![layer_norm_03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/layer_norm_03.png)

如图BN是对“我”和“今”做均值和方差，而LN是对“我爱中国共产党”这句话做均值和方差。

### 3 前馈神经网络

前馈神经网络中的Feed Forward是全连接层。

## Decoder

![decoder01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/decoder01.png)

1. 多头注意力机制
2. 交互层

### 1 多头注意力机制

mask: 简单来说就是掩盖当前单词和之后的单词

![decoder02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/decoder02.png)

为什么需要mask：如图Decoder在预测单词YOU时，应该只能看到前面的单词，不应该输入单词YOU和后面的单词NOW。

### 2 交互层

![decoder03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/decoder03.png)

Ecoder的输出和每一个Decoder做交互

![decoder04](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/decoder04.png)

具体的交互是Ecoder的输出生成K、V矩阵，Decoder生成Q矩阵，然后根据多头注意力机制进行计算

![decoder05](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Transformer/decoder05.png)

虚线代表K、V矩阵的输出

#### 参考资源

- [Transformer从零详细解读](https://www.bilibili.com/video/BV1Di4y1c7Zm)
- [公众号DASOU资源汇总](https://docs.qq.com/doc/DYWdjYmNUbmpmUXlv)
- [The Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)
