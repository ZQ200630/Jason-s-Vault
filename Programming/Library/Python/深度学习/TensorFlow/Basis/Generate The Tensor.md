---
tags: 深度学习 PyTorch Python
---
# Generate The Tensor

```python
tf.constant(value, dtype=tf.int8)
tf.convert_to_tensor(numpy, dtype=tf.int8)

tf.zeros(dim)
tf.ones(dim)

tf.fill(dim, val)

1 dim: 3
2 dim: [3,5]
3 dim: [3, 5, 8]
    
tf.random.normal(dim, mean=, stddev=) 
[[Generate]] the Gaussian distribution
tf.random.truncated_normal(dim, mean=, stddev=) 
[[Truncated]] normal distribution, guarantee all data distribute in 2 stddev.
```