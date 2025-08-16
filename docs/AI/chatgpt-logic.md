# ChatGPT的底层逻辑

Type: AI

### ChatGPT进行学习的四个阶段

1. 学习文字接龙

一个不完整的句子，通过GPT模型生成一个可能的字。GPT模型是根据互联网上大量的语料来得出结果，这样的好处是不用人工标注。

![1](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt01.png)

这个字可以是GPT模型学习了大量的语料，生成一个概率分布得到的。

![2](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt02.png)

文字接龙也可以应用到回答问题。

![3](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt03.png)

但是实际上GPT模型会得到很多结果，它并不知道哪个好。

![4](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt04.png)

1. 人类老师引导文字接龙的方向

找人来思考想问GPT的问题，并人工提供正确答案。如回答台湾最高的山是哪座？人工标注为玉山。

我们不需要穷尽所有问题，只要告诉GPT人类的偏好。

1. 模仿人类老师的喜好

通过人工标注的方式我们告诉了GPT哪个答案更好，然后我们需要训练一个模型（Teacher Model）来模拟人类对答案的偏好，这个模型通过打分的方式来得出答案。

![5](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt05.png)

1. 用增强式学习向模拟老师学习

模拟老师（Teacher Model）会对GPT回答的不好的问题给出低分，这个分值就是增强式学习中的Reward，然后调整参数，以得到最大的Reward。

![6](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt06.png)

最后训练出来的GPT模型能给出较好的答案，这样就得到了最终的ChatGPT。

![7](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Chatgpt/ppt07.png)