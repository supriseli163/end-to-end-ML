# Gabor滤波器
## 介绍
我们知道，傅里叶变化是一种信号处理的有效工具，可以帮助我们将图像从时域转换到频域，并提取到时域上不易提取的特征。
但是经过傅里叶变换后，图像在不同位置的频度特征往往混合在一起，但是Gabor滤波器却可以抽取空间局部频度特征，是一种有效的纹理检测工具。
在图像处理中，通常会进行特征提取，而特征提取通常利用滤波器对图像进行操作，提取图像中各种有用的统计信息如颜色、纹理、朝向等。滤波器的作用
随着其特点的不同而不同。它主要分析的是图像在某一特定区域的特定方向上是否有特定频率的内容

![Image](http://ww1.sinaimg.cn/large/535663c3gw1f4au7dsla3j20ug0bjwfq.jpg)
## 如何生成一个Gabor滤波器
在二维空间中，使用一个三角函数(如正弦函数)与一个高斯函数叠加我们就得到了一个Gabor滤波器.
![Image](http://ww3.sinaimg.cn/large/535663c3gw1f4auqgsxedj209n0jutal.jpg)
## Gabor核函数
二维Gabor函数由一个高斯函数和一个余弦函数相乘得出。
![Image](http://ww3.sinaimg.cn/large/535663c3gw1f4av9098mkj21bz096t9x.jpg)
Gabor滤波器特别适合于纹理表示和辨别，在空间域中，2D Gabor滤波器是由正弦平面调制的高斯核函数，其脉冲相应定义为一个正弦波(对于二维Gabor滤波器是平面波)乘以高斯函数。
因为乘法卷积性质(卷积定理)，由于乘法卷积性质,Gabor滤波器的脉冲响应是傅里叶变换是调和函数的傅立叶变换和高斯函数的傅里叶变换的卷积。该滤波器具有表示正交方向的实部和虚部。
两个分量可以构成复数。

由上述可知，gabor特征是一种可以用来描述图像的纹理信息和特征，此外，Gabor小波对于图像的边缘敏感，能够提供良好的方向选择和尺度选择特性，Gabor滤波器
可以提取不同方向上的纹理信息。Gabor滤波器对于光照变化不敏感，能够提供对光照变化良好的适应性，能容忍一定程度的图像旋转和变形，对光照、姿势具有一定的鲁棒性。

## 参考资料
[1] https://blog.csdn.net/yanzi6969/article/details/42736855
[2] https://blog.csdn.net/yanzi6969/article/details/42736855
[3] http://matlabserver.cs.rug.nl/edgedetectionweb/web/edgedetection_params.html
[4] 