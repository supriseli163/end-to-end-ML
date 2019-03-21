# 超像素(superpixel)
```java
超像素概念是2003年Xiaofeng Ren提出和发展起来的图像分割技术，是指具有相似纹理、颜色、亮度等
特征构成的有一定视觉意义的不规则像素块。它利用像素之间特征的相似性将像素分组，用少量的超像素代替大量的像素
来表达图片特征，很大成都上降低了图像后处理的复杂度，所以通常作为分割算法的预处理步骤。已经广泛用于图像分割、姿势估计、
目标跟踪、目标识别等计算机视觉应用。
```
几种常见的分割方法包括:
Graph-based、NCut、Turbopixel、Qucik-shift、Graph-cuta、Graph-cutb以及SLIC
其中,SLIC(Simple linear iterative clustering)即简单线性迭代聚类。
几种常见的超像素分割方法以及效果对比如下：
![Image](https://img-blog.csdn.net/20150506134210315)
## SLIC
SLIC(Simple linear iterativeclustering)是2010年提出的一种思想简单、实现方便的算法，
将彩色图像转化为CIELAB颜色空间和XY坐标下的5维特征向量，然后对5维特征向量构造距离度量标准，
对图像像素进行局部聚类的过程。
SILC主要优点如下：
1. 生成的超像素如同细胞一样紧凑整齐，领域特征比较容易表达，这样基于像素的方法可以比较容易的改造为基于超像素的方法
2. 不仅可以分割彩色图，也可以见人分割灰度图
3. 需要设置的参数非常少，默认情况下只需要设置一个预分割的超像素的数量
4. 相比其他的超像素分割方法，SLIC在运行速度、生成超像素的紧凑度、轮廓保持方面都比较理想。
## 效果图
![Image](https://ask.qcloudimg.com/http-save/yehe-3443194/kbseqql039.jpeg?imageView2/2/w/1620)
k=64时，第一轮迭代，效果图
![Image](https://ask.qcloudimg.com/http-save/yehe-3443194/ijavvwtzo9.jpeg?imageView2/2/w/1620)
第20轮迭代
![Image](https://ask.qcloudimg.com/http-save/yehe-3443194/ebteulf34x.jpeg?imageView2/2/w/1620)
k=128时，效果图
第一轮迭代
![Image](https://ask.qcloudimg.com/http-save/yehe-3443194/t0nzbd9uup.jpeg?imageView2/2/w/1620)

```python
import math
from skimage import io, color
import numpy as np
from tqdm import trange


class Cluster(object):
    cluster_index = 1

    def __init__(self, h, w, l=0, a=0, b=0):
        self.update(h, w, l, a, b)
        self.pixels = []
        self.no = self.cluster_index
        self.cluster_index += 1

    def update(self, h, w, l, a, b):
        self.h = h
        self.w = w
        self.l = l
        self.a = a
        self.b = b

    def __str__(self):
        return "{},{}:{} {} {} ".format(self.h, self.w, self.l, self.a, self.b)

    def __repr__(self):
        return self.__str__()


class SLICProcessor(object):
    @staticmethod
    def open_image(path):
        """
        Return:
            3D array, row col [LAB]
        """
        rgb = io.imread(path)
        lab_arr = color.rgb2lab(rgb)
        return lab_arr

    @staticmethod
    def save_lab_image(path, lab_arr):
        """
        Convert the array to RBG, then save the image
        """
        rgb_arr = color.lab2rgb(lab_arr)
        io.imsave(path, rgb_arr)

    def make_cluster(self, h, w):
        return Cluster(h, w,
                       self.data[h][w][0],
                       self.data[h][w][1],
                       self.data[h][w][2])

    def __init__(self, filename, K, M):
        self.K = K
        self.M = M

        self.data = self.open_image(filename)
        self.image_height = self.data.shape[0]
        self.image_width = self.data.shape[1]
        self.N = self.image_height * self.image_width
        self.S = int(math.sqrt(self.N / self.K))

        self.clusters = []
        self.label = {}
        self.dis = np.full((self.image_height, self.image_width), np.inf)

    def init_clusters(self):
        h = self.S / 2
        w = self.S / 2
        while h < self.image_height:
            while w < self.image_width:
                self.clusters.append(self.make_cluster(h, w))
                w += self.S
            w = self.S / 2
            h += self.S

    def get_gradient(self, h, w):
        if w + 1 >= self.image_width:
            w = self.image_width - 2
        if h + 1 >= self.image_height:
            h = self.image_height - 2

        gradient = self.data[w + 1][h + 1][0] - self.data[w][h][0] + \
                   self.data[w + 1][h + 1][1] - self.data[w][h][1] + \
                   self.data[w + 1][h + 1][2] - self.data[w][h][2]
        return gradient

    def move_clusters(self):
        for cluster in self.clusters:
            cluster_gradient = self.get_gradient(cluster.h, cluster.w)
            for dh in range(-1, 2):
                for dw in range(-1, 2):
                    _h = cluster.h + dh
                    _w = cluster.w + dw
                    new_gradient = self.get_gradient(_h, _w)
                    if new_gradient < cluster_gradient:
                        cluster.update(_h, _w, self.data[_h][_w][0], self.data[_h][_w][1], self.data[_h][_w][2])
                        cluster_gradient = new_gradient

    def assignment(self):
        for cluster in self.clusters:
            for h in range(cluster.h - 2 * self.S, cluster.h + 2 * self.S):
                if h < 0 or h >= self.image_height: continue
                for w in range(cluster.w - 2 * self.S, cluster.w + 2 * self.S):
                    if w < 0 or w >= self.image_width: continue
                    L, A, B = self.data[h][w]
                    Dc = math.sqrt(
                        math.pow(L - cluster.l, 2) +
                        math.pow(A - cluster.a, 2) +
                        math.pow(B - cluster.b, 2))
                    Ds = math.sqrt(
                        math.pow(h - cluster.h, 2) +
                        math.pow(w - cluster.w, 2))
                    D = math.sqrt(math.pow(Dc / self.M, 2) + math.pow(Ds / self.S, 2))
                    if D < self.dis[h][w]:
                        if (h, w) not in self.label:
                            self.label[(h, w)] = cluster
                            cluster.pixels.append((h, w))
                        else:
                            self.label[(h, w)].pixels.remove((h, w))
                            self.label[(h, w)] = cluster
                            cluster.pixels.append((h, w))
                        self.dis[h][w] = D

    def update_cluster(self):
        for cluster in self.clusters:
            sum_h = sum_w = number = 0
            for p in cluster.pixels:
                sum_h += p[0]
                sum_w += p[1]
                number += 1
                _h = sum_h / number
                _w = sum_w / number
                cluster.update(_h, _w, self.data[_h][_w][0], self.data[_h][_w][1], self.data[_h][_w][2])

    def save_current_image(self, name):
        image_arr = np.copy(self.data)
        for cluster in self.clusters:
            for p in cluster.pixels:
                image_arr[p[0]][p[1]][0] = cluster.l
                image_arr[p[0]][p[1]][1] = cluster.a
                image_arr[p[0]][p[1]][2] = cluster.b
            image_arr[cluster.h][cluster.w][0] = 0
            image_arr[cluster.h][cluster.w][1] = 0
            image_arr[cluster.h][cluster.w][2] = 0
        self.save_lab_image(name, image_arr)

    def iterate_10times(self):
        self.init_clusters()
        self.move_clusters()
        for i in trange(20):
            self.assignment()
            self.update_cluster()
            name = 'Elegent_Girl_M{m}_K{k}_loop{loop}.jpg'.format(loop=i, m=self.M, k=self.K)
            self.save_current_image(name)


if __name__ == '__main__':
    for k in [64, 128, 256, 1024]:
        p = SLICProcessor('800.jpg', k, 30)
        p.iterate_10times()
```

# 参考文章
[1] https://www.cnblogs.com/Imageshop/p/6193433.html
[2] https://cloud.tencent.com/developer/article/1348843 超像素分割源码