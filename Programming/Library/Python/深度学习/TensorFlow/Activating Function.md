---
tags: 深度学习 TensorFlow Python
---
# Activating Function

```python
tf.nn.softmax(tf.matmul(X,W) + B)  
#将输出变为概率的形式输出，通常与交叉熵损失函数一起使用。
```

$$y_i = \frac{e^i}{\sum_j{e^j}}, 0 < y_i < 1$$

```python
tf.nn.sigmoid(tf.matmul(X,W) + B)  
#将输出变为概率形式输出，通常与平方差函数一起使用。
```

$$
f(x) = \frac{1}{1 + e^{-x}} ,0 < f(x) < 1
$$

```python
tf.nn.tanh(tf.matmul(X,W) + B)  
[[中心位置为0]]，对称，与平方差损失函数一起使用。
```

$$
f(x) = \frac{\sinh{x}}{\cosh x}=\frac{e^x - e^{-x}}{e^x + e^{-x}}=\frac{1-e^{-2x}}{1 + e^{-2x}}, -1 < f(x) < 1
$$

```python
tf.matmul(X,W) + B  
#线性激活函数
```

$$
f(x) = x , -\infty < f(x) < \infty
$$

```python
tf.nn.relu(tf.matmul(X,W) + B)  
#类似于线性激活函数
```

$$
f(x) = max(x, 0),0 < f(x) < \infty
$$

```python
tf.nn.softplus(tf.matmul(X,W) + B)  
[[relu]]函数的平滑版本
```

$$
f(x) = log(1 + e^x),0 < f(x) < \infty
$$

```python
tf.nn.softsign(tf.matmul(X,W) + B) 
#并非处处可导
```

$$
f(x) = \frac{x}{abs(x) + 1}, -1 < f(x) < 1
$$

```python
tf.nn.leaky_relu(tf.matmul(X,W) + B)  
[[relu]]激活函数的改进版本，解决部分输入会落到硬饱和区，导致对应的权重无法更新的问题
```

$$
f(x) = max(x, leak*x), -\infty < f(x) < \infty
$$

```python
tf.nn.elu(tf.matmul(X,W) + B)  
[[leaky_relu]]左边非线性改变
```

$$
ELU(x) = \begin{cases}
&x&if(x > 0) \\
&\alpha (e^x - 1) \quad&if(x \le 0)
\end{cases}
$$

```python
tf.nn.selu(tf.matmul(X,W) + B) 
#最近的自归一化网络中提出
```

$$
SELU(x) = \lambda\begin{cases}
&x&if(x > 0) \\
&\alpha (e^x - 1) &if(x \le 0)
\end{cases}, \lambda > 1
$$