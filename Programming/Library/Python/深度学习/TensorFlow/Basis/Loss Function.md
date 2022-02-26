---
tags: 深度学习 PyTorch Python
---
# Loss Function

```python
tf.losses.categorical_crossentropy(y_, y)
```

$$H(y_, y) = - \sum y\_ * lny$$

```python
# y_ should be the one-hot code
# y should indicate the possibility of all outcomes
# y will be normalized if the some of the possibility larger than 1

tf.nn.softmax_cross_entropy_with_logits(y_, y) 
# Softmax and cross entropy were calculated simultaneously

tf.nn.l2() 
[[use]] l2 regularization
```