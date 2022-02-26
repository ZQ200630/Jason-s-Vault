---
tags: 深度学习 PyTorch Python
---
# PyTorch

## 绘图代码

```python
def use_svg_display():
	# 用矢量图表示
	display.set_matplotlib_formats('svg')
def set_figsize(figsize=(3.5, 2.5)):
	use_svg_display()
	# 设置图尺寸
	plt.rcParams['figure.figsize'] = figsize

set_figsize()

plt.scatter(features[:, 1].numpy(), labels.numpy(), 1);
```

## 读取数据

```python
def data_iter(batch_size, features, labels):
	num_examples = len(features)
	indices = list(range(num_examples))
	random.shuffle(indices) 
	for i in range(0, num_examples, batch_size):
		j = torch.LongTensor(indices[i: min(i + batch_size, num_examples)])
		yield features.index_select(0, j), labels.index_select(0, j)

batch_size = 10
for X, y in data_iter(batch_size, features, labels):
	print(X, y)
	break
```

### 简单方法

```python
import torch.utils.data as Data
batch_size = 10
# 将训练数据特征和标签组合
dataset = Data.TensorDataset(features, labels)
# 随机读取小批量
data_iter = Data.DataLoader(dataset, batch_size, shuffle=True)

```