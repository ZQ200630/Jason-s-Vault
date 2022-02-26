---
tags: 深度学习 PyTorch Python Keras
---
# Print Weight

```python
np.set_printoptions(threshold=np.inf) # Do not use ... show the middle data
model.trainable_variables # Return the trainable param in model

np.set_printoptions(threshold=np.inf)
print(model.trainable_variables)
file=open('./weights.txt', 'w')
for v in model.trainable_variables:
    file.write(str(v.name) + '\n')
    file.write(str(v.shape) + '\n')
    file.write(str(v.numpy()) + '\n')
file.close()
```