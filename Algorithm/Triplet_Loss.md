# Triplet Loss
在人脸识别中，Triplet loss被用来进行人脸嵌入训练。如果对triplet loss很陌生，可以看一下[吴恩达的课程](https://www.coursera.org/lecture/convolutional-neural-networks/triplet-loss-HuUtN)。
Triplet Loss实现起来并不容易，特别是想要把它加到tensorflow的计算图中。

## Triplet Loss 简介
Triplet Loss可以翻译为三元组损失，其中的三元也即是如下图的Anchor、Negative、Positive，如下图所示通过Triplet Loss
的学习后使得Positive元和Anchor元之间的距离最小，而和Negative之间距离最大。其中Anchor为训练数据集中随机选取的一个样本，Positive为和Anchor属于同一类的样本，
而Negative则为Anchor不同类的样本。
![Triplet Loss](https://img-blog.csdn.net/20170814123212482)


# Triplet loss和triplets挖掘
## 为什么不用softmax
google的论文[FaceNet:A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/abs/1503.03832)
最早将triplet loss应用到人脸识别中。他们提出了一种实现人脸嵌入和在线triplet挖掘的方法。

# 参考文献
- [1].https://blog.csdn.net/hustqb/article/details/80361171
- [2].https://blog.csdn.net/qq_28659831/article/details/80805291
