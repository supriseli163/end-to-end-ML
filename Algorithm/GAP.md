# Global Average Pooling
Global Average Pooling第一次出现在论文NetWork in NetWork中，后来又很多工作延续使用了GAP,实验证明:Global
Average Pooling确实可以提高CNN效果。
## Tradition Pooling Methods
 要想真正的理解Global Average Pooling，首先要了解深度网络中常见的pooling方式，以及全连接层。
 众所周知CNN网络中常见的结构是:卷积、池化、和激活。卷积层是CNN网络的核心，激活函数帮助网络获得非线性特征，而池化的作用体现在降采样、保留显著特征、降低特征纬度，
 增大Kernel的感受野。深度网络越往后面越能捕捉到物体的语义信息，这种语义信息是建立在较大的
 ## Full Connected Layer
 全连接一直是CNN分类网络的标配结构，一般在券链接后会有激活函数来做分类，假设这个激活函数是一个多分类softmax，那么全连接网络的作用就是将最后一层卷积得到的feature map stretch成向量，对这个向量做乘法，最终降低其维度。
 然后输入到softmax层中得到对应的每个类别的得分。全链接层如此zyao
 ## Global Average Pooling
 有了上面的基础，再来看看Global Average Pooling。既然全连接网络可以使Feature Map的维度减少，进而输入到softmax，但是又会造成过拟合，可以用pooling来代替全连接。
 ![Image](https://leanote.com/api/file/getImage?fileId=596cddb4ab644114ba001f4e)
 
 ## 参考文献
 [1] https://blog.csdn.net/williamyi96/article/details/77530995
 [2] https://www.sohu.com/a/206341878_473283
 