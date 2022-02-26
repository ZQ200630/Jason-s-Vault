---
tags: 深度学习 PyTorch Python Keras
---
# Data Processing

```python
image_gen_train = tf.keras.preprocessing.images.ImageDataGenerator(
	rescale=
    rotation_range=
    width_shift_range=
    height_shift_range=
    horizontal_flip=
    zoom_range=
)

# x_train = x_train.reshape(x_train.shape[0],28,28,1)
image_gen_train.fit(x_train)
model.fit(image_gen_train.flow(x_train, y_train, batch_size=32))
```