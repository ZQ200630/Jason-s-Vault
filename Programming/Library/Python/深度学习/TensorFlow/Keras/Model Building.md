---
tags: 深度学习 PyTorch Python Keras
---
# Model Building

```python
model = tf.keras.models.Sequential([struct of net])
1. tf.keras.layers.Flatten() 
[[make]] a n-D tensor to 1-D vector
2. tf.keras.layers.Dense(num, activation="function", kernel_regularizer="regular")
# activation: relu, softmax, sigmoid, tanh， softsign, softplus, selu, hard_sigmoid, linear
# kernel_regularizer: tf.keras.regularizers.l1(), tf.keras.regularizers.l2(),keras.regularizers.l1_l2(l1=, l2=)
3. tf.keras.layers.Conv2D(filters = kernelNum, kernel_size = size, strides = step, padding = "valid" or "same") 
[[the]] Convolutional layer
4. tf.keras.layers.LSTM() 
[[LSTM]] RNN layer
```