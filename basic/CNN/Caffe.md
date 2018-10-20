# Caffe
Convolutional Architetcure for Fast Feature Embedding.

# Caffe的几个重要概念
Blobs: Caffe使用blos的结构来存储、交换和处理网络中正向的反向迭代是的数据和导数信息，有统一的接口类型
Layers: Layers是Caffe模型和计算的基本单元
Nets: Net是一系列layers和其连接的集合
Forward and Backward: Caffe的两条计算生命线，Forward产生loss和输出结果；Backward产生反向梯度
Loss: 计算真实值和预测误差值，知道下一步工作，由loss定义待学习的任务
Slover:solver协调模型的优化
Layer Catalogue:学习caffe中构建先进模型所需的各种层的功能
Interface: Caffe的命令行、python、和 Matlab版接口


# Blob概念图
![Image](https://img-blog.csdn.net/20170401233715771?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvRXJyb3JzX0luX0xpZmU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
# Caffe官网
https://blog.csdn.net/errors_in_life/article/details/68948841
