---
tags: 深度学习 PyTorch Python Keras
---
# Basic

```python
padding='valid' # 不使用全零填充
padding='same' # 使用全零填充
Conv2D(filters=6, 5, padding='valid', activation='sigmoid'， strides=1)
[[kernel_size]]: 如果宽高相等可以只写一个值，否则使用元祖
[[strides]] 滑动步长，默认为1
```