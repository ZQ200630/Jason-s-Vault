---
tags: 深度学习 PyTorch Python Keras
---
# Save Model Weight

```python
cp_callback = tf.keras.callbacks.ModelCheckpoint(
	filepath = path,
    save_weights_only=True/False,
    save_best_only=True/False)
history = model.fit(callbacks=[cp_callback])
[[PS]]: if the save_best_only be true, you should add valid_data
```