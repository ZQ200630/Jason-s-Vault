---
tags: 深度学习 PyTorch Python Keras
---
# Model Fit

```python
model.fit(x, y, batch_size= , epochs= , 
         validation_data=(测试集输入特征，测试集标签),
         validation_split=从训练集划分多少比例给测试集
         validation_freq=多少次epoch测试一次)
```