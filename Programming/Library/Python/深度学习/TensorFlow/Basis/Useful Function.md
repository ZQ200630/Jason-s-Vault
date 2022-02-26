---
tags: 深度学习 PyTorch Python
---
# Useful Function

```python
tf.cast(tensor, dtype=) 
[[cast]] tensor
tf.reduce_min(tensor) 
[[calculate]] the min value in the tensor
tf.reduce_max(tensor) 
[[calculate]] the max value in the tensor

tf.reduce_mean(tensor, axis=) 
[[axis]]=0 means along the row, axis = 1 means along the colom. If not assign the axis, operate all the elements.

tf.Variable() 
# Mark the tensor as a trainable variable, the variable will records the gradient info in the back propagation.
tf.Variable(tf.random.normal([m,n], mean=0, stddev=1)) 

tf.add(A, B)
tf.subtract(A, B)
tf.multiply(A, B)
tf.divide(A, B)
tf.pow(A, B)

[[SP]]: The operation in A & B will broadcast like numpy
[[SP]]: The dtype between A & B must be same

tf.square(A)
tf.sqrt(A) 
# SP: The dtype in A must be a float

tf.matmul(A, B)

tf.data.Dataset.from_tensor_slices((features, labels)) # match the features and labels, features and labels should be a tense of the same length, The type in tensor should also be same

# features = tf.constant([1, 2, 3, 4])
# labels = tf.constant([4, 3, 2, 1])
# dataset = tf.data.Dataset.from_tensor_slices((features, labels))
# for element in dataset:
#     print(element) # the element is a tuple, which has features and it's corresponding labels

with tf.GradientTape() as tape:
    grad = tape.gradient(function, w) 
		# Calculate the gradient of the function against the variable.
    [[PS]]: the dtype of function and w must be float
    
    
for index, element in enumerate(list):
    print(i, element) 
		# mapping the index and the element in list

tf.one_hot(data, depth=) 
[[use]] one_hot code to return the result, the depth indicate how many class you want to show.

# tf.assign(var, cons) # assign the var the constant value
# tf.assign_add(var, cons)
# tf.assign_sub(var, cons) 
# The method below only adapt to TF1

tf.Variable().assign(num/tensor) 
# assign the value
tf.Variable().assign_sub(num/tensor) 
# Self - subtracting a variable
tf.Variable().assign_add(num/tensor) 
# Self - adding a variable

tf.argmax(tensor, axis=) 
# return the max value index along the axis 

tf.where(conditional statement, A, B) 
# return A if conditional statement is true else B

np.random.RandomState.rand(dim) 
[[if]] dim is null, return a scalar, a random value between 0 - 1
```

### Random Value

```python
np.random.normal(mean, stddev, size) 
[[return]] a normal distribution matrix
np.random.rand(size) 
[[return]] a union distribution matrix between 0 - 1
np.random.RandomState(seed=1)
```

### Array Merging

```python
np.vstack((A, B) 
# merge A and B vertically
np.hstack((A, B)) 
# merge A and B horizontally
```

### Grid Generator

```python
x, y = np.mgrid[start : end : step, start : end : step] 
# generate the grid of two variable
x.ravel()  
# make the x as a 1-D array
np.c_[x.ravel(), y.ravel()] 
# match x and y
```