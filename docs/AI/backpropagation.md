# 反向传播算法

## 公式推导

![1](https://gitlab.com/iknowledge/BlogImage/-/raw/main/AI/backpropagation.png)

初始条件：
- 神经元中不考虑偏置与激活函数的影响
- 输入：$x_1=40,x_2=80$
- 期望输出：$y=60$
- 损失函数：$\frac{1}{2}(y-y^\prime)^2$
- 学习率：1e-5
- 第一层参数全初始化为0.5
-  第二层参数全初始化为1

一、前向传播：

$$
z_1 = w_{11}^\prime x_1 + w_{21}^\prime x_2 = 0.5\times 40+0.5\times 80=60 \\
z_2 = w_{12}^\prime x_1 + w_{22}^\prime x_2 = 0.5\times 40+0.5\times 80=60 \\
z_3 = w_{13}^\prime x_1 + w_{23}^\prime x_2 = 0.5\times 40+0.5\times 80=60 \\
y^\prime=w_{11}^{\prime\prime}z_1+w_{21}^{\prime\prime}z_2+w_{31}^{\prime\prime}z_3=1\times 60+1\times 60+1\times 60=180
$$

二、计算误差（损失值）

$$
Loss=\frac{1}{2}(y-y^\prime)^2=\frac{1}{2}(60-180)^2=7200
$$

三、反向传播

3.1 计算梯度

- 损失函数的梯度

$$
\frac{\partial L}{\partial y^\prime}=\frac{1}{2}\times 2\times(y-y^\prime)\times -1=y^\prime-y=180-60=120
$$

- 第2层的梯度

$$
\frac{\partial L}{\partial w_{11}^{\prime\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial w_{11}^{\prime\prime}}=120\times z_1=120\times 60=7200 \\
\frac{\partial L}{\partial w_{21}^{\prime\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial w_{21}^{\prime\prime}}=120\times z_2=120\times 60=7200 \\
\frac{\partial L}{\partial w_{31}^{\prime\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial w_{31}^{\prime\prime}}=120\times z_3=120\times 60=7200
$$

- 第1层的梯度

$$
\frac{\partial L}{\partial w_{11}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_1}\frac{\partial z_1}{\partial w_{11}^{\prime}}=120\times w_{11}^{\prime\prime}\times x_1=120\times 1\times 40=4800 \\
\frac{\partial L}{\partial w_{12}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_2}\frac{\partial z_2}{\partial w_{12}^{\prime}}=120\times w_{21}^{\prime\prime}\times x_1=120\times 1\times 40=4800 \\
\frac{\partial L}{\partial w_{13}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_3}\frac{\partial z_1}{\partial w_{13}^{\prime}}=120\times w_{31}^{\prime\prime}\times x_1=120\times 1\times 40=4800
$$

$$
\frac{\partial L}{\partial w_{21}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_1}\frac{\partial z_1}{\partial w_{21}^{\prime}}=120\times w_{11}^{\prime\prime}\times x_2=120\times 1\times 80=9600 \\
\frac{\partial L}{\partial w_{22}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_2}\frac{\partial z_2}{\partial w_{22}^{\prime}}=120\times w_{21}^{\prime\prime}\times x_2=120\times 1\times 80=9600 \\
\frac{\partial L}{\partial w_{23}^{\prime}}=\frac{\partial L}{\partial y^\prime}\frac{\partial y^\prime}{\partial z_3}\frac{\partial z_1}{\partial w_{23}^{\prime}}=120\times w_{31}^{\prime\prime}\times x_2=120\times 1\times 80=9600 \\
$$

3.2 梯度更新

$$
w = w-\eta\frac{\partial L}{\partial w} \\
\partial w_{11}^{\prime\prime}=\partial w_{11}^{\prime\prime}-\eta\frac{\partial L}{\partial w_{11}^{\prime\prime}}=1-10^{-5}\times 7200=0.928 \\
\partial w_{21}^{\prime\prime}=\partial w_{21}^{\prime\prime}-\eta\frac{\partial L}{\partial w_{21}^{\prime\prime}}=1-10^{-5}\times 7200=0.928 \\
\partial w_{31}^{\prime\prime}=\partial w_{31}^{\prime\prime}-\eta\frac{\partial L}{\partial w_{31}^{\prime\prime}}=1-10^{-5}\times 7200=0.928
$$

$$
\partial w_{11}^{\prime}=\partial w_{11}^{\prime}-\eta\frac{\partial L}{\partial w_{11}^{\prime}}=0.5-10^{-5}\times 4800=0.452 \\
\partial w_{12}^{\prime}=\partial w_{12}^{\prime}-\eta\frac{\partial L}{\partial w_{12}^{\prime}}=0.5-10^{-5}\times 4800=0.452 \\
\partial w_{13}^{\prime}=\partial w_{13}^{\prime}-\eta\frac{\partial L}{\partial w_{13}^{\prime}}=0.5-10^{-5}\times 4800=0.452 \\
\partial w_{21}^{\prime}=\partial w_{21}^{\prime}-\eta\frac{\partial L}{\partial w_{21}^{\prime}}=0.5-10^{-5}\times 9600=0.404 \\
\partial w_{22}^{\prime}=\partial w_{22}^{\prime}-\eta\frac{\partial L}{\partial w_{22}^{\prime}}=0.5-10^{-5}\times 9600=0.404 \\
\partial w_{23}^{\prime}=\partial w_{23}^{\prime}-\eta\frac{\partial L}{\partial w_{23}^{\prime}}=0.5-10^{-5}\times 9600=0.404
$$

## 代码实现

### 简单实现

```python
# 定义输入值和期望输出
x_1 = 40.0
x_2 = 80.0
expected_output = 60

'''
初始化
'''
# 定义权重
w_1_11 = 0.5
w_1_12 = 0.5
w_1_13 = 0.5
w_1_21 = 0.5
w_1_22 = 0.5
w_1_23 = 0.5

w_2_11 = 1.0
w_2_21 = 1.0
w_2_31 = 1.0

'''
前向传播
'''
z_1 = x_1 * w_1_11 + x_2 * w_1_21
z_2 = x_1 * w_1_12 + x_2 * w_1_22
z_3 = x_1 * w_1_13 + x_2 * w_1_23
y_pred = z_1 * w_2_11 + z_2 * w_2_21 + z_3 * w_2_31

print('前向传播预测值为：', y_pred)

# 计算损失函数值（L2损失）
loss = 0.5 * (expected_output - y_pred) ** 2
print('当前任务的loss值为：', loss)

'''
开始计算梯度
'''
# 计算输出层关于损失函数的梯度
d_loss_predicted_output = -(expected_output - y_pred)
print('损失函数的梯度为：', d_loss_predicted_output)

# 计算权重关于损失函数的梯度
d_loss_w_2_11 = d_loss_predicted_output * z_1
d_loss_w_2_21 = d_loss_predicted_output * z_2
d_loss_w_2_31 = d_loss_predicted_output * z_3

d_loss_w_1_11 = d_loss_predicted_output * w_2_11 * x_1
d_loss_w_1_21 = d_loss_predicted_output * w_2_11 * x_2
d_loss_w_1_12 = d_loss_predicted_output * w_2_21 * x_1
d_loss_w_1_22 = d_loss_predicted_output * w_2_21 * x_2
d_loss_w_1_13 = d_loss_predicted_output * w_2_31 * x_1
d_loss_w_1_23 = d_loss_predicted_output * w_2_31 * x_2

# 使用梯度下降法更新权重
learning_rate = 1e-5
w_2_11 -= learning_rate * d_loss_w_2_11
w_2_21 -= learning_rate * d_loss_w_2_21
w_2_31 -= learning_rate * d_loss_w_2_31

w_1_11 -= learning_rate * d_loss_w_1_11
w_1_12 -= learning_rate * d_loss_w_1_12
w_1_13 -= learning_rate * d_loss_w_1_13
w_1_21 -= learning_rate * d_loss_w_1_21
w_1_22 -= learning_rate * d_loss_w_1_22
w_1_23 -= learning_rate * d_loss_w_1_23

'''
前向传播
'''
z_1 = x_1 * w_1_11 + x_2 * w_1_21
z_1 = x_1 * w_1_12 + x_2 * w_1_22
z_1 = x_1 * w_1_13 + x_2 * w_1_23

y_pred = z_1 * w_2_11 + z_2 * w_2_21 + z_3 * w_2_31
print('Final：', y_pred)

loss = 0.5 * (expected_output - y_pred) ** 2
print('当前的loss值为：', loss)
```

### 封装成函数

```python
def forward_propagation(layer_1_list, layer_2_list):
    w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23 = layer_1_list
    w_2_11, w_2_21, w_2_31= layer_2_list
    z_1=x_1 * w_1_11 + x_2 * w_1_21
    z_2=x_1 * w_1_12 + x_2 * w_1_22
    z_3=x_1 * w_1_13 + x_2 * w_1_23
    y_pred = z_1 * w_2_11 + z_2 * w_2_21 + z_3 * w_2_31
    return y_pred

def compute_loss(y_true, y_pred):
    loss = 0.5 * (y_true - y_pred)**2
    return loss

def backward_propagation(layer_1_list, layer_2_list, learning_rate):
    w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23 = layer_1_list
    w_2_11, w_2_21, w_2_31=layer_2_list
    z_1 = x_1 * w_1_11 + x_2 * w_1_21
    z_2 = x_1 * w_1_12 + x_2 * w_1_22
    z_3 = x_1 * w_1_13 + x_2 * w_1_23
    # 计算输出层关于损失函数的梯度
    d_loss_predicted_output =-(y_true-y_pred)
    # 计算权重关于损失函数的梯度
    d_loss_w_2_11 = d_loss_predicted_output * z_1
    d_loss_w_2_21 = d_loss_predicted_output*z_2
    d_loss_w_2_31 = d_loss_predicted_output * z_3
    
    d_loss_w_1_11 = d_loss_predicted_output * w_2_11* x_1
    d_loss_w_1_21 = d_loss_predicted_output * w_2_11 * x_2
    d_loss_w_1_12 = d_loss_predicted_output * w_2_21 * x_1
    d_loss_w_1_22 = d_loss_predicted_output * w_2_21 * x_2
    d_loss_w_1_13 = d_loss_predicted_output * w_2_31 * x_1
    d_loss_w_1_23 = d_loss_predicted_output * w_2_31 * x_2

    # 使用梯度下降法更新配置
    w_2_11 -= learning_rate * d_loss_w_2_11
    w_2_21 -= learning_rate * d_loss_w_2_21
    w_2_31 -= learning_rate * d_loss_w_2_31
    w_1_11 -= learning_rate * d_loss_w_1_11
    w_1_12 -= learning_rate * d_loss_w_1_12
    w_1_13 -= learning_rate * d_loss_w_1_13
    w_1_21 -= learning_rate * d_loss_w_1_21
    w_1_22 -= learning_rate * d_loss_w_1_22
    w_1_23 -= learning_rate * d_loss_w_1_23
    
    layer_1_list = [w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23]
    layer_2_list = [w_2_11, w_2_21, w_2_31]
    return layer_1_list, layer_2_list

def parm_init():
    # 初始化定义权重
    w_1_11 = 0.5
    w_1_12 = 0.5
    w_1_13 = 0.5
    w_1_21 = 0.5
    w_1_22 = 0.5
    w_1_23 = 0.5
    
    w_2_11 = 1.0
    w_2_21 = 1.0
    w_2_31 = 1.0

    layer_1_list = [w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23]
    layer_2_list = [w_2_11, w_2_21, w_2_31]
    return layer_1_list, layer_2_list

if __name__ == '__main__':
    # 定义输入值和期望输出
    x_1 = 40.0
    x_2 = 80.0
    y_true = 60.0
    learning_rate = 1e-5
    
    epoch = 100
    
    '''
    初始化
    '''
    # 初始化定义权重
    layer_1_list, layer_2_list = parm_init()
    
    for i in range (epoch):
        # 正向传播
        y_pred = forward_propagation(layer_1_list, layer_2_list)
        loss = compute_loss(y_true, y_pred)
        print(f"第{i}次 预测值为：", y_pred, " 误差为：", loss)
        # 计算损失
        layer_l_list, layer_2_list = backward_propagation(layer_1_list, layer_2_list, learning_rate)
```

### pytorch版

```python
import torch
import torch.optim as optim


def forward_propagation(x_1, x_2, layer_1_list, layer_2_list):
    w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23 = layer_1_list
    w_2_11, w_2_21, w_2_31 = layer_2_list
    z_1 = x_1 * w_1_11 + x_2 * w_1_21
    z_2 = x_1 * w_1_12 + x_2 * w_1_22
    z_3 = x_1 * w_1_13 + x_2 * w_1_23
    y_pred = z_1 * w_2_11 + z_2 * w_2_21 + z_3 * w_2_31
    return y_pred


def compute_loss(y_true, y_pred):
    loss = 0.5 * (y_true - y_pred) ** 2
    return loss


def backward_propagation(layer_1_list, layer_2_list, optimizer):
    # 清零梯度
    optimizer.zero_grad()
    # 反向传播
    loss.backward()
    # 使用优化器更新权重
    optimizer.step()
    # 返回更新后的权重
    return layer_1_list, layer_2_list


def parm_init():
    # 初始化定义权重
    w_1_11 = torch.tensor(0.5, requires_grad=True)
    w_1_12 = torch.tensor(0.5, requires_grad=True)
    w_1_13 = torch.tensor(0.5, requires_grad=True)
    w_1_21 = torch.tensor(0.5, requires_grad=True)
    w_1_22 = torch.tensor(0.5, requires_grad=True)
    w_1_23 = torch.tensor(0.5, requires_grad=True)

    w_2_11 = torch.tensor(1.0, requires_grad=True)
    w_2_21 = torch.tensor(1.0, requires_grad=True)
    w_2_21 = torch. tensor(1.0, requires_grad=True)
    w_2_31 = torch. tensor(1.0, requires_grad=True)

    layer_1_list = [w_1_11, w_1_12, w_1_13, w_1_21, w_1_22, w_1_23]
    layer_2_list = [w_2_11, w_2_21, w_2_31]
    return layer_1_list, layer_2_list


if __name__ == "__main__":
    # 定义输入值和期望输出
    x_1 = torch.tensor([40.0])
    x_2 = torch.tensor([80.0])
    y_true = torch.tensor([60.0])
    learning_rate = 1e-5

    epoch = 100
    '''
    初始化
    '''
    # 初始化定义权重
    layer_1_list, layer_2_list = parm_init()

    # 使用SGD优化器进行权重更新
    optimizer = optim.SGD(layer_1_list + layer_2_list, lr=learning_rate)

    for i in range(epoch):
        # 正向传播
        y_pred = forward_propagation(x_1, x_2, layer_1_list, layer_2_list)
        # 计算损失
        loss = compute_loss(y_true, y_pred)
        # 反向传播
        layer_1_list, layer_2_list = backward_propagation(
            layer_1_list, layer_2_list, optimizer)
        print(f"第{i}次 预测值为：", y_pred.item(), " 误差为：", loss.item())
```

#### 参考资源

- [神经网络反向传播：手撕公式+代码复现](https://www.bilibili.com/video/BV1a14y167vh)
