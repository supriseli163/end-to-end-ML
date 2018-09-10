# 1.研究综述
https://www.zhihu.com/people/zhengzhedong/activities
1. 作者：罗浩
2. 前言： 行人重识别(Person Re-identification)也称行人再识别，是利用计算机视觉技术判断图像或者视频
序列中是否存在特定行人的技术，广泛被认为是一个图像检索的子问题，给定一个监控行人图像，检索跨设备下的该行人图像。

在监控视频中，由于相机分辨率和拍摄角度的缘故，通常服务得到质量非常高的人脸图片，当人脸识别失效的情况下，ReID就成为了一个非常
重要的替代品技术。ReId有一个非常重要的特性就是跨摄像头，所以学术论文里评价的时候，是要检索出不同摄像下的相同行人图片。ReId已经在
学术界研究多年，但直到最近几年随着深度学习的发展，才取得了非常巨大的突破。因此，本文介绍一些近几年基于深度学习的ReID工作，由于精力有限并不能涵盖所有工作，只能介绍几篇
代表性的工作。
主要由一下几类：
```text
1.基于表征学习的ReId方法
2.基于度量学习的方法
3.基于局部特征的ReId方法
4.基于视频序列的ReId方法
5.基于GAN造图的方法
```
## 1.基于表征学习的方法
基于表征学习(Representation learning)的方法是一类非常常用的行人重识别方法,这主要得益于深度学习，尤其是卷积神经网络(Convolutional neural newtwork,CNN)
的快速发展。由于CNN可以自动从原始图像数据中根据任务需求自动提取出表征特征(Representation),所以有些研究者把行人重识别问题看作分类(Classification/identification)
问题；
1. 分类问题是指利用行人的ID或者属性来训练标签来训练模型
2. 验证问题是指输入一对(两张)行人图片，让网络来学习这两张图片是否属于同一行人。

论文[1]利用classification/identification loss和verification loss来训练网络,其网络示意图如下图所示。网络输入为若干对行人图片，包括子分类网络(classification subnet)
和验证子网络(verification subnet)。分类子网络对图片进行ID预测，根据预测的ID来计算分类误差损失。验证子网络融合两张图片的特征，判断这两张图片是否属于同一个行人，该子网络
实质上等于一个二分类网络，结果足够数据的训练，再次输入一张测试图片，网络将自动提取出一个特征，这个特征用于行人重识别任务。
![Image](//Users/jinxi.li/code/github/end-to-end-ML/文献/行人辨识相关的文献/Person_ReId_1.jpg)

但也有论文认为光靠行人的ID信息不足以学习出一个泛化能力若足够强的模型，在这些人的工作中，它们额外标注了行人图片的属性特征，例如性别、头发、衣着等属性。
通过引入行人属性标签，模型不但要准确地预测出行人ID，还要预测出各项正确的行人属性，这大大增加了模型泛化的能力，多数论文也显示这种方法是有效的，下图是
，从图中可以看出，网络输出的特征不仅用于预测行人的ID信息，还用于预测各项行人属性。通过结合ID损失和属性损失能够提高网络的泛化能力。
如今依然有大量的工作是基于表征学习的，表征学习也称为ReID领域的一个非常重要的baseline，并切表征学习的方法比较鲁棒，训练比较稳定，训练结果也比较容易复现。
但是个人的实际经验感觉表征学习容易在数据集的domain上过拟合，并且当训练ID增加到一定程度的时候会显得比较乏力。
# 2.基于度量学习的ReId方法
度量学习(Metric Learning)是广泛用于图像检索领域的一种方法。不同于表征学习,度量学习旨在通过网络学习出两张图片的相似度，在行人重识别问题上，具体为同一行人的不同图片相似度大于不同行人的不同图片。
最后网络的损失函数使得相同行人图片(正样本对)的距离尽可能小，不同行人图片(负样本对)的距离尽可能大。常用的度量学习损失方法有对比损失方法有对比损失(Contrastive loss)
、三元组损失(Triplet loss)、四元组损失(Quadruplet loss)[9]、难样本采样三元组损失(Triplet hard loss with batch hard mining,TriHard loss)[10]，边界挖掘损失(Margin sample mining loss,MSML)[11]。
首先，假如有两张输入图片I1和I2，通过网络的前馈我们可以得到它们归一化后的特征向量 f_{I_1} 和 f_{I_2}.
我们定义这两张图片的特征向量的欧式距离为：
 $$\begin{cases}
 a_1x+b_1y+c_1z=d_1\\
 a_2x+b_2y+c_2z=d_2\\
 a_3x+b_3y+c_3z=d_3\\
 \end{cases}
 $$
 
#4.基于视频序列的ReID的方法
目前单帧的ReId研究还是主流，因为相对来说数据集比较小，哪怕一个单GPU的PC做一次实验也不会花太长时间，但是通常单帧图像的信息是有限的，因此有很多工作集中在利用视频
序列来进行行人重识别方法的研究[17-24]。基于视频序列的方法最主要的不同点就是这类方法不仅考虑了图像的内容信息，还考虑了帧与帧之间的运动信息等。
基于单帧图像的方法主要思想是利用CNN来提取图像的空间特征，而基于视频序列的方法主要思想是利用CNN来提取空间特征的同时利用递归循环网络(Recurrent neural networks,RNN)
来提取时序特征，上图就是非常典型的思路，网络输入为图像序列。每张图像都经过一个共享的CNN提取出图像空间内容特征，之后这些特征向量被输入到一个RNN网络去提取最终的特征。
最终的特征融合了但帧图像的内容特征和帧与帧之间的运动特征。而这个特征用于代替前面单帧方法的图像来训练网络。
视频序列类的代表方法之一是累计运动背景网络(Accumulative motion context network,AMOC)[23].AMOC输入的包括原始的图像序列和提取的光流序列。通常提取光流信息需要用到传统的光流提取算法，
但是这些算法计算耗时，并且无法与深度学习网络兼容。为了能够得到一个自动提取光流的网络,作者首先训练了一个运动信息网络(Motion network,Moti Nets)。这个运动网络输入为原始的图像序列，
标签为传统方法提取的光流序列。如下图所示，原始的图像序列显示在第一排，提取的光流序列显示在第二排。网络有三个光流预测的输出，分别为Pred1，Pred2,Pred3。这三个输出能够预测三个不同尺度的光流图，最后网络融合了三个
尺度上的光流预测输出来得到最终光流图，预测的光流序列在第三排显示。通过最小化预测光流图和提取光流图的误差，网络能够提取出较为准确的运动特征。


 # 基于局部特征的ReId方法
 早期的ReId研究大家还主要关注点在全局的global feature上，就是用整图得到一个特征向量进行图像检索，但是后来大家逐渐发现全局特征遇到了瓶颈，
 于是开始渐渐地研究起局部的local feature。常用的提取局部特征的思路主要有图像切块、利用骨架关键点定位以及姿态矫正等。
 - (1)图片切块是一种很常见的提取局部特征方式[12]。如下图所示，图片被垂直等分为若干份，因为垂直切割更符合我们对人体识别的直观感受，所以行人重识别领域很少用到水平切割。
 之后，被分割好的若干块图像按照顺序送到一个长短时记忆网络(Long short term memory network, LSTM)，最后的特征融合了所有的图像块的局部特征。
 但是这种缺点在于对图像对其的要求比较高，如果两幅图像没有上下对齐，那么很可能出现头和上身对比的现象，
 
 # 5.基于GAN造图的ReId方法
 ReId有一个非常大的问题就是数据获取困难，截止CVPR18 dedaline截稿之前，最大的ReId数据集也就小几千个ID，几万张图片(序列假定只算一张)。
 因此在ICCV17 GAN造图做ReID挖了第一个坑之后，就有大量的GAN的工作涌现，尤其是CVPR18 dedaline截稿之后arxiv出现了好几篇paper.
 论文[25]是第一篇用GAN做ReID的文章，发表在ICCV17会议，虽然论文比较简单,但是作为挖坑鼻祖引出一系列很好的工作。如下图，这篇论文生成的图像质量还不是很高，
 甚至可以用很惨来形容，论文提出了一个问题就是由于图像是随机生成的，也就是说是没有可以标注的label可以用的。为了解决这个问题，论文提出了一个标签平滑的方法。
 实际操作也很简单，就是把label vector每一个元素的值都取一样，满足加起来为1。反正也看不出属于那个人，那就一碗水端平。生成的图像作为训练数据加入到训练之中。
 由于当时的baseline还不像现在这么高，所以效果还是很明显的，至少数据的量多了过拟合能避免很多。
 
 作者：罗浩.ZJU
 链接：https://zhuanlan.zhihu.com/p/31921944
 来源：知乎
 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
 
 # 参考文献
- [1] Mengyue Geng, Yaowei Wang, Tao Xiang, Yonghong Tian. Deep transfer learning for person reidentification[J]. arXiv preprint arXiv:1611.05244, 2016.
- [2] Yutian Lin, Liang Zheng, Zhedong Zheng, YuWu, Yi Yang. Improving person re-identification by attribute and identity learning[J]. arXiv preprint arXiv:1703.07220, 2017.
- [3] Liang Zheng, Yi Yang, Alexander G Hauptmann. Person re-identification: Past, present and future[J]. arXiv preprint arXiv:1610.02984, 2016.
- [4] Tetsu Matsukawa, Einoshin Suzuki. Person re-identification using cnn features learned from combination of attributes[C]//Pattern Recognition (ICPR), 2016 23rd International Conference on. IEEE, 2016:2428–2433.
- [5] Rahul Rama Varior, Mrinal Haloi, Gang Wang. Gated siamese convolutional neural network architecture for human re-identification[C]//European Conference on Computer Vision. Springer, 2016:791-808.
- [6] Florian Schroff, Dmitry Kalenichenko, James Philbin. Facenet: A unified embedding for face recognition and clustering[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition.2015:815-823.
- [7] Hao Liu, Jiashi Feng, Meibin Qi, Jianguo Jiang, Shuicheng Yan. End-to-end comparative attention networks for person re-identification[J]. IEEE Transactions on Image Processing, 2017.
- [8] De Cheng, Yihong Gong, Sanping Zhou, Jinjun Wang, Nanning Zheng. Person re-identification by multichannel parts-based cnn with improved triplet loss function[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016:1335-1344.
- [9] Weihua Chen, Xiaotang Chen, Jianguo Zhang, Kaiqi Huang. Beyond triplet loss: a deep quadruplet network for person re-identification[J]. arXiv preprint arXiv:1704.01719, 2017.
- [10] Alexander Hermans, Lucas Beyer, Bastian Leibe. In defense of the triplet loss for person reidentification[J]. arXiv preprint arXiv:1703.07737, 2017
- [11] Xiao Q, Luo H, Zhang C. Margin Sample Mining Loss: A Deep Learning Based Method for Person Re-identification[J]. 2017.
- [12] Rahul Rama Varior, Bing Shuai, Jiwen Lu, Dong Xu, Gang Wang. A siamese long short-term memory architecture for human re-identification[C]//European Conference on Computer Vision. Springer, 2016:135–153.
- [13] Liang Zheng, Yujia Huang, Huchuan Lu, Yi Yang. Pose invariant embedding for deep person reidentification[J]. arXiv preprint arXiv:1701.07732, 2017.
- [14] Haiyu Zhao, Maoqing Tian, Shuyang Sun, Jing Shao, Junjie Yan, Shuai Yi, Xiaogang Wang, Xiaoou Tang. Spindle net: Person re-identification with human body region guided feature decomposition and fusion[C]. CVPR, 2017.
- [15] Longhui Wei, Shiliang Zhang, Hantao Yao, Wen Gao, Qi Tian. Glad: Global-local-alignment descriptor for pedestrian retrieval[J]. arXiv preprint arXiv:1709.04329, 2017.
- [16] Zhang, X., Luo, H., Fan, X., Xiang, W., Sun, Y., Xiao, Q., ... & Sun, J. (2017). AlignedReID: Surpassing Human-Level Performance in Person Re-Identification. arXiv preprint arXiv:1711.08184.
- [17] Taiqing Wang, Shaogang Gong, Xiatian Zhu, Shengjin Wang. Person re-identification by discriminative selection in video ranking[J]. IEEE transactions on pattern analysis and machine intelligence, 2016.38(12):2501–2514.
- [18] Dongyu Zhang, Wenxi Wu, Hui Cheng, Ruimao Zhang, Zhenjiang Dong, Zhaoquan Cai. Image-to-video person re-identification with temporally memorized similarity learning[J]. IEEE Transactions on Circuits and Systems for Video Technology, 2017.
- [19] Jinjie You, Ancong Wu, Xiang Li, Wei-Shi Zheng. Top-push video-based person reidentification[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition.2016:1345–1353.
- [20] Xiaolong Ma, Xiatian Zhu, Shaogang Gong, Xudong Xie, Jianming Hu, Kin-Man Lam, Yisheng Zhong. Person re-identification by unsupervised video matching[J]. Pattern Recognition, 2017. 65:197–210.
- [21] Niall McLaughlin, Jesus Martinez del Rincon, Paul Miller. Recurrent convolutional network for videobased person re-identification[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016:1325–1334.
- [22] Rui Zhao, Wanli Oyang, Xiaogang Wang. Person re-identification by saliency learning[J]. IEEE transactions on pattern analysis and machine intelligence, 2017. 39(2):356–370.
- [23] Hao Liu, Zequn Jie, Karlekar Jayashree, Meibin Qi, Jianguo Jiang, Shuicheng Yan, Jiashi Feng. Video based person re-identification with accumulative motion context[J]. arXiv preprint arXiv:1701.00193,2017.
- [24] Song G, Leng B, Liu Y, et al. Region-based Quality Estimation Network for Large-scale Person Re-identification[J]. arXiv preprint arXiv:1711.08766, 2017.
- [25] Zheng Z, Zheng L, Yang Y. Unlabeled samples generated by gan improve the person re-identification baseline in vitro[J]. arXiv preprint arXiv:1701.07717, 2017.
- [26] Zhong Z, Zheng L, Zheng Z, et al. Camera Style Adaptation for Person Re-identification[J]. arXiv preprint arXiv:1711.10295, 2017.[27] Wei L, Zhang S, Gao W, et al. Person Transfer GAN to Bridge Domain Gap for Person Re-Identification[J]. arXiv preprint arXiv:1711.08565, 2017.
- [28] Qian X, Fu Y, Wang W, et al. Pose-Normalized Image Generation for Person Re-identification[J]. arXiv preprint arXiv:1712.02225, 2017.