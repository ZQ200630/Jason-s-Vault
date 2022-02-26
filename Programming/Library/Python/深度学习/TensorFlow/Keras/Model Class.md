---
tags: 深度学习 PyTorch Python Keras
---
# Model Class

```python
class MyModel(Model):
    def __init__(self):
        super(Mymodel, self).__init__()
    def call(self, x):
        return y
    
model = MyModel()
# __init__() 定义所需网络结构块
# call() 写出前向传播

class IrisModel(Model):
    def __init__(self):
        super(IrisModel, self).__init__()
        self.d1 = Dense(3)
    def call(self, x):
        y = self.d1(x)
        return y
model = IrisModel()
```