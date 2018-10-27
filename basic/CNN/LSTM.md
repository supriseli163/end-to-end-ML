# LSTM的原理和实现  长短期记忆网络
LSTM(long short term memory)，即我们所称呼的LSTM，是为了解决长期以来问题而专门设计出来的，所有的RNN都具有一种重复神经网络模块的链式形式。
在标准的RNN中，这个重复结构模块只有一个非常简单的结构，例如一个tanh层。
![Image](https://ws1.sinaimg.cn/large/005BVyzmly1fotn5cyzypj30jg07a74x.jpg)
LSTM同样是这样的结构，但是重复的模块拥有一个不同的结构，不同于一个单一神经网络层，这里有四个，以一种非常特殊的方式进行交互。
![Image](https://ws1.sinaimg.cn/large/005BVyzmly1fotnatxsm7j30jg07bjsi.jpg)
图中使用的各种元素的图标
![Image](https://ws1.sinaimg.cn/large/005BVyzmly1fotnatxsm7j30jg07bjsi.jpg)
# LSTM核心思想
LSTM的关键在于细胞的状态，整个绿色的图表示的是一个Cell,和穿过细胞的那条水平线.
细胞状态类似于传送带，直接在整个链上允许，只有一些少量的线性交互。信息在上面流传保持不变会很容易。
![Image](https://ws1.sinaimg.cn/large/005BVyzmly1fotnwop43ej30jg060aab.jpg)

LSTM解决长期依赖的问题：
1> 例如,在看电影的时候，某些情节的推断需要依赖很久以前的一些细节
很多其他的任务也一样，很可惜随着时间间隔不断增大时，RNN会丧失学习到连接如此远的信息的能力。
也就是说，记忆容量有限，一本书从头到尾一字不漏的去记，肯定离得越远的东西忘的越多。
LSTM是RNN一种，大体结构改造过，
