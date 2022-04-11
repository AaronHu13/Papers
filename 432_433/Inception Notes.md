
```
V1
增加了网络的深度和宽度:深度确实是增加了,通过增加宽度,实现了多通道的应用?从而取得了更加优秀的检测效果,
1*1 卷积核的应用:
1: 增加网络的深度,增强其非线性行
2: 降维,这样子在增加网络的宽度时依然可以保证网络的性能

设计动机:
增加网络的尺寸可以增加网络的性能->通过增加网络的深度或者增加网络的宽度->会造成过拟合以及难以训练,且计算量会以平方的级别进行增长

解决方法:引入稀疏->同一层的网络可以有更多不同的选择(对比ResNet,后者其实是在深度上增加了更多的可选择性)

论文写得很严谨:目前这样子的结果是否是最优的,难说,一切有待后人进行考量和验证

全文思路:找到一个最优的子模块,重复从而取得全局的最优

不难看出,在网络传播的过程中,更抽象的信息将会在更后面被捕捉到,从而在更深的网络中,将会需要更多的5*5的卷积核,但这将会导致参数的爆炸->Inception 需要进行降维

结合需要"sparse"的要求,最终网络结构设计成如下结果:在参数没有进行整合计算时,尽可能的保持稀疏,从而达到更好地学习效果;另一方面,当整合参数并可能导致参数爆炸时,适当进行降维,即引入1*1的卷积核(同时1*1卷积核的引入也可以增加非线性性 ),以此达到两者的平衡.从而有了Inception现在的网络结构

最后鉴于训练内存的限制,只有在更深层的网络结构里才开始使用Inception的module,而在前面的网络中还是使用的传统的conv stack的模式.这不是技术上的必要而是限制于当前的硬件

此外,这篇文章也提到了多尺度学习:Furthermore, the design follows the practical intuition that visual information should be processed at various scales and then aggregated so that the next stage can abstract features from the different scales simultaneously.

这和R-CNN系列的anchor有相似之处
```
```
V2
Batch Normalization - 
用更高的学习率进行学习,同时不需要考虑初始化,在取得相同准确率的情况下,训练的步骤降为原来的1/14,在不追求快速训练的情况下,准确率取代了当前的state of art
```

```
V3
1. 卷积分解:与VGGNet 相似,5*5 分解为两个3*3,从而实现降维
2. 非对称卷积分解: instead of using 3*3, we can use 3*1 and 1*3 to reach the same result with less parameters.这一思想在MobileNet中也得到的充分的运用,效果相当好.相较之下,将3*3分解成两个2*2只减少了11%的参量.且同时这样的转换在网络的开始部分效果并不明显,只有在更深的地方才会取得非常良好的效果.

3. 1*1 卷积降维为何不会造成明显的准确率损失:推测是在
We hypothesize that the reason for that is the strong correlation between adjacent unit results in much less loss of information during dimension reduction, if the outputs are used in a spatial aggregation context. Given that these signals should be easily compressible, the dimension reduction even promotes faster learning.


```

Formally, denoting the desired underlying mapping as H(x), we let the stacked nonlinear layers fit another mapping of F(x) = H(x) - x. The original mapping is recast into F(x) + x. They hypothesize that it is easier to optimize the residual mapping than to optimize the original, unreferenced mapping.

