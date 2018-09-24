# DenseNet
## DenseNet简介
    在CVPR2017上，康奈尔大学博士后黄高博士(Gao Huang)、清华大学本科生刘壮(Zhuang Liu)、Facebook人工智能研究院研究科学家Laurens van der Maaten以及康奈尔大学计算机系教授
    Kilian Q.Weinberger所做论文<<Densely Connected Convolutional Networks>>当选CVPR2017最佳论文，
    与苹果首篇公开论文<<Learning From Simulated and Usupervised Images through Adversial Training>>共获得这一殊荣。
## DenseNet的基本结构
 DenseNet是一种具有密集连接的卷积神经网络。在该网络中，任何两层之间都有直接的连接，也就是说，网络每一层的输入都是前面所有层输出的并集，而该层所学习的特征图也会
 被直接传给其后面所有层作为输入。下面是DenseNet的一个示意图。
 ![Image](http://img.mp.itc.cn/upload/20170803/a472b95c1e4740aea2f5d88003bcd833_th.jpg)
 
 ## 参考文章
 [1] https://blog.csdn.net/Bryan__/article/details/77337109