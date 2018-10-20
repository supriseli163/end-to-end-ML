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
    

# 参考文章
[1] https://blog.csdn.net/qq_31531635/article/details/72232651

