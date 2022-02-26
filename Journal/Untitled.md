# Introduction

1. 介绍语义分割

 

 Semantic segmentation, also known as semantic label- ing, is one of the fundamental and challenging tasks in re- mote sensing image understanding, whose goal is to assign pixel-wise semantic class labels for a given image. In partic- ular, semantic segmentation in very high resolution (VHR) aerial images plays an increasingly significance for its wides- pread applications, such as road extraction [21], urban plan- ning [45] and land cover classification [23].
 
 
 对于语义分割网络的处理在近十年来发生了翻天覆地的变化，从十年前的传统的贝叶斯分类，决策树，主成分分析，支持向量机，随机森林，条件随机场，Boosting等算法到五年以前的DCNN的广泛应用，再到FCN作为DCNN 应用于Semantic Segmentation 的鼻祖，以此为基础延伸了众多的CNN网络的改良，再到后来Self Attention逐渐广泛应用以及2020年开始Transformer的爆火。
 
  
  早期的网络是疯狂下采样再上采样，这样会导致图像的边界模糊，精细度不足

To capture long-range dependencies, such as correlation coefficients between long-distance pixels, Chen et al. [6] proposed atrous spatial(空洞卷积，增加感受野) pyramid pooling (ASPP) with multi- scale dilation rates to aggregate contextual information（金字塔池化，多尺度辨析）. Zhao et al. [46] further introduced the pyramid pooling module (PPM) to represent the feature map via multiple regions with different sizes. 
Problems: 终归使用的是卷积，只能获取局部的信息。

Nevertheless, the context aggrega- tion methods above are still unable to extract global contex- tual information, that is, it is unsatisfactory to cover global receptive fields by stacking and aggregating convolutional layers.

Furthermore, in order to generate dense and pixel-wise contextual information, Non-local Neural Networks [38] uti- lizes a self-attention mechanism, which enables a single fea- ture from any position to perceive features from all other positions. It can be seen as a matter of feature reconstruc- tion, that is, the feature representation of each position is a weighted sum of all other counterparts. DANet [10] in- troduces spatial-wise and channel-wise attention modules to enrich the feature representations. Besides, several works [16, 17, 15] improve the efficiency of the self-attention mech- anism to some extent. However, pixel-wise attention ap- proaches still need to generate a dense attention map to mea- sure the relationships between each pixel-pair, which has a high computation complexity and occupies a huge number of GPU memory. Recent works [8, 17] have shown the fact that information redundancy is not conducive to feature represen- tations. What’s more, attention-based methods are restricted to the perspective of space and channel, ignoring category- based information, which is a key factor for semantic seg- mentation task. The category-based information is directly related to the last classifier of the network. In general, lack of category-based information and huge computation com- plexity of self-attention mechanism are two tough problems and will be elaborated below.

Self-Attention 模型的出现: 可以建立本像素与区域内任意像素的联系，通过Key Value匹配， 也有一部分问题，计算量过大


近来的数种Transformer网络表现出了强大的泛化能力与全局信息的提取能力，在远程遥感图像中，既有只占有几个像素的特别小的目标，也有几乎占据了半张图片的大尺度目标，因此当卷积流网络想要准确分辨出这些细节的时候就会比较费力。

In this paper, 比较 不同的网络在不同数据集上面的表现，并将比较它们的参数, 分类IOU与准确度, Induction Speed等参数, 均匀衡量新老网络的优缺点




 参数量

 
3. 介绍高分辨率Remote Sensing特点
4. 介绍应用

# Related Work

# Methodology
## Data Collection

ISPRS / LoveDA

## Preprocessing

RandomCrop 0.75 cat——max_ratio
RandomFlip 0.5prob
PhotoMetricDistortion


## Hyperparameters

80000 iter
optimizer = dict(type='SGD', lr=0.01, momentum=0.9, weight_decay=0.0005)



# Results



# Discussions



# Conclusions

