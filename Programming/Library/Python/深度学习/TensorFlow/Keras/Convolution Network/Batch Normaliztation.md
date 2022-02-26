---
tags: 深度学习 PyTorch Python Keras
---
# Batch Normaliztation

```python
tf.keras.layers.BatchNormalization()

model = tf.keras.models.Sequential([
    Conv2D(),
    BatchNormalization(),
    Activation('relu'),
    MaxPool2D(),
    Dropout(0.2)
])
```