---
tags: 深度学习 PyTorch Python Keras
---
# Read Model Weight

```python
load_weights(path)

checkpoint_save_path = "***.ckpt"
if os.path.exists(checkpoint_save_path + '.index'):
    print('-------------load the model-----------------')
    model.load_weights(checkpoint_save_path)
```