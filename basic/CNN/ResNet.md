# ResNet解析
ResNet在2015年被提出，在ImageNet比赛classification任务上获得了第一名，因为它"简单与实用"并存，
之后很多方法都建立在ResNet50或者ResNet101的基础上完成的、检测、分割、识别等领域都纷纷使用ResNet，
Alpha zero也使用了ResNet，所以可见ResNet确实很好用。
Residual Network深度残差网络,可以说是过去几年中计算机视觉和深度学习领域最具开创性的工作。ResNet使得训练数百甚至数千层成为可能，且在这种情况下仍能
展现出优越的性能。
因其强大的表征能力，除图像分类外，包括目标检测和人脸识别在内的许多计算机视觉应用都得到了性能提升。

## 重新审视ResNet
根据泛逼近定理(universal approximation theorem)，只要给足够的容量，单层的前馈网络
也足以表示任何函数，但是，该层可能非常庞大，网络和数据易出现过拟合。因此，研究界普遍任务
网络架构需要更多层。
![Image](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=1073801752,1119132767&fm=173&app=25&f=JPEG?w=640&h=261&s=8CA67D338793487358F5A8DE000080B1)
## 增加网络深度导致性能下降
在ResNet出现之前就有几种方法来应对梯度消失问题，例如在中间层添加了一个辅助损失作为额外的监督，但其中没有一种方法真正解决了这个问题。
ResNet的核心思想是引入一个所谓的[恒等快捷连接]identity shortcut connection，直接跳过一个或多个层，如下图所示
![Image](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=89694971,1272391370&fm=173&app=25&f=JPEG?w=640&h=339&s=7AAC3C62998F40CA5C7450DF0000C0B1)
残差块
![Image](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2068577801,554247547&fm=173&app=25&f=JPEG?w=572&h=1314&s=803060321D9144CA4E5550C8010030B0)
# 参考文章
[1] https://baijiahao.baidu.com/s?id=1598536455758606033&wfr=spider&for=pc