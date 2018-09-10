# Detect to Track and Tack to Detect
## 文章概览
- 使用了一个简单的卷积网络模型(ConvNet)在视频序列中同时实现多目标的跟踪和检测
- 构建了一个新颖的损失函数,包括用于单帧检测的多任务损失和用于多帧间跟踪回归损失
- 引入相关特征用于代表同一目标在不同帧图片中同时出现以此到达跟踪的目的
- 检测和跟踪相互辅助，同时产生高精度的检测和跟踪性能
- 提出在多帧中同时进行目标检测和跟踪任务，其中检测部分使用R-FCN框架，跟踪部分则将基于相关和回归的跟踪思想融入到上述检测框架中
- 网络在ImageNet VID上进行训练和检测，网络结构简单且得到了当前最好的实验效果
# 相关工作
- 当前视频中实现跟踪和检测的大部分方法都是基于tracking by detection思路，即检测器检测目标，然后用跟踪器跟踪目标，当跟踪可信度较低时，
用检测器辅助捕获目标，这种跟踪检测框架主要被基于单帧的目标检测方法所支配
- 由于各种视频数据库(典型的为VID数据库)的出现，基于视频的目的检测方法备受关注，不用于tracking by detection框架，为视频中跟踪和检测问题提供了另一种解决思路
- VID数据库特点：数据量大(共高达130W张图片)，运动模糊的图像多(更接近真实情况)，分辨率低(视频中的图片往往币静态图片分辨率低)，存在大量目标被遮挡的情况且目标姿态丰富(可以提高算法的鲁棒性)
- 当前在VID数据库上实现视频检测的绝大部分性能好的算法基本都附带复杂的后处理逻辑
- 本文选用R-FCN为主体的框架,R-FCN优点：全卷积结构，速度快分辨率高适合跟踪任务
- 本文跟踪部分思想借鉴于Fully-convolutional siamese跟踪框架和100 FPS deep regression跟踪网络
- 新视频目标检测数据集: A large High-Precision Human-Annotated Data Set for Object Detection in Video(2017)，每个图片中仅有一个物体标注。
# 本文算法概览
- 使用end-to-end的方式训练用于同时进行跟踪和检测的全卷积网络
- 损失函数为多任务损失函数，由跟踪损失和检测损失构成
- 主体网络采用ResNet-101网络，网络输出为多帧图片，提取出的特征为检测和跟踪共享
- 为实现跟踪，在ResNet-101的不同尺度特征层进行帧间特征的交叉相关操作，即第t帧的第n,n+1，n+2层特征分别于第t+N帧的第n,n+1,n+2层特征做相关计算
- 检测部分，在最终特征层使用ROI Pooling特征进行分类和bbox回归操作
- 跟踪部分，在最终习惯后的特征层使用ROI Pooling特征进行帧间的bbox变化回归估计
- 实验表明，加入跟踪loss后可以提升特征学习质量，更有利于目标的检测
- 扩大帧间间隔后，可实现视频中快速目标跟踪检测
- 多帧输入，ResNet-101主干结构，R-FCN检测网络，跟踪检测共享卷积特征
- 损失=检测分类损失+检测回归损失+跟踪回归损失
- RoI Pooling:同R-FCN一样，结合RPN+position-sensitive score map，得到目标类别得分和bbox回归值
- RoI Tracking:输入为两帧特征(包括卷积中间层和positives-sensitive score map)的相关操作后的结果，通过RPN(使用第t帧的RPN)指示经过RoI Tracking输出坐标变化关系
- 网络改变:同R-FCN相同，对ResNet-101中conv5的stride由2改为1，同时使用dilated convolution方法增加感受视野

https://blog.csdn.net/zhangjunhit/article/details/78225967
# 参考文献
- [1].https://mp.weixin.qq.com/s/gGKuGUKEWkosk44MoDFGrA?
- [2].https://blog.csdn.net/aiqiu_gogogo/article/details/78240834
- [3].http://deeplearning.net/tutorial/s