# GoogleNet
    Inception(也称GoogleNet)是2014年Christian Szegedy提出的一种全新的深度学习结构，在这之前的AlxeNet、VGG等结构都是通过增大网络的深度(层数)
来获得更好的训练效果，但层数的增加会带来很多负作用，比如overfit、梯度消失、梯度爆炸等。
Inception的提出则从另外一种角度来提升训练结构，能更高效的利用计算资源，在相同的计算量下能提取到更多的特征，从而提升训练结果。
## 提出背景
    始于LeNet-5，一个有着标准的堆叠式卷积层冰带有一个或多个全连接层的卷积神经网络。通常使用dropout来针对过拟合问题。
    为了提出一个更深的网络，GoogleNet做到了22层，利用interception结构，这个结构很好地利用了网络中的计算资源，并且在不断增加计算负载的情况下，
    增加网络深度和宽度。同时，为了优化网络质量，采用Hebbian原理和多尺度处理,GoogleNet在分类和检测上都取得了不错的效果。
## 相关工作
    GoogleNet参考Robust object recognition with cortex-like mechanisms中利用固定的多个Gabor滤波器来进行多尺度降维和限制网络尺寸的作用。
    GoogleNet参考Rich feature hierachies for accurate object detection and semantic segmentation，即R-CNN将整个检测任务分为两个自问题的做法，即首先
    利用底层特征如颜色，文本等来进行提取与类别无关的proposals，然后将这些proposal放入CNN中进行训练来确定类别信息的做法，GoogleNet也借鉴这种方式，并对两个阶段都进行了改进，第一个阶段
    使用多边框检测，第二个阶段则是使用更好的CNN网络结构。
## 基本思想以及过程
```java
GoogleNet提出最直接提升深度神经网络的方法是增加网络的尺寸，包括宽度和深度，深度也就是网络中的层数，宽度是指每层中所用到的神经元的个数。但是这种简单直接的解决方式存在的两个重大的缺点。
1.网络尺寸的增加也意味着参数的额增加，也就是使得网络更加容易过拟合
2.计算资源的增加
    因此想到将全联接的方式改为稀疏连接来解决这两个问题，由Provalbe bounds for learning some deep representations。提到数据集的概率分布由大稀疏的深度神经网络表达时，网络拓扑结构可
由逐层分析与输出高度相关的上一层的激活值和聚类神经元的相关统计信息来优化，但是这有非常多的限制条件，因此提出运用Hebbian原理，它
可以使得上述想法在少量限制条件下就变得实际可行。
    通常全联接是为了更好地优化并行计算，而稀疏连接是为了打破对称来改善学习，传统常常利用卷积来利用空间上的稀疏性，但卷积在网络
    的早期层中的与patches的连接也是稠密连接，因此考虑到能不能在滤波器层面上利用稀疏性，而不是神经元上，但是在非均匀稀疏数据结构上进行数值
    计算效率很低，，并且查找和缓存未定义的开销很大，而且对计算的基础设施要求过高，因此考虑到将稀疏矩阵聚类成相对稠密子空间来倾向于对稀疏矩阵
```


![Image](https://img-blog.csdn.net/20170516000934768?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE1MzE2MzU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Inception结构的主要思想在于卷积视觉网络中一个优化的局部稀疏结构怎么样能由一系列易获得的稠密子结构来近似和覆盖，上面提到网络拓扑结构是由逐层
分析上一层的相关统计信息并聚集到一个高度相关的单元组中。
![Image](https://img-blog.csdn.net/20170516001127442?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE1MzE2MzU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

上图为GoogleNet的网络框图细节，其中"#3X3 reduce"，"#5X5 reduce"代表在3X3,5X5卷积操作之前使用1X1卷积的数量，输入图像为224X224X3，且都
进行了零均值化的预处理操作，所有的降维层也都是使用了ReLU非线性激活函数。
![Image](https://img-blog.csdn.net/20170516001209599?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE1MzE2MzU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如上图用到了辅助分类器，inception Net有22层深，除了最后一层的输出，其中间节点的分类效果也很好，因此在Inception Net中，还使用到了辅助分类节点(auxiliary classfiers)，即将中间某一层的输出用作分类，并按一个较小的权重(0.3)加到最终分类结构中，
这相当于做了模型融合，同时给网络增加了反向传播的梯度信号，也提供了额外的正则化，对于整个inception Net的训练很有益。
![Image](https://img-blog.csdn.net/20170516002609260?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE1MzE2MzU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 参考文章
[1] https://blog.csdn.net/qq_31531635/article/details/72232651

