Batch Normalization层

Batch Normalization是2015年由Sergey Ioffe和Chritian Szegedy在论文[]中提出。在最新的CNN结构设计，如ResNet，GoogLeNet Inception V4等，BN层都包含于其中。

BN层的核心思想是将卷机网络中间层的输出都进行一次高斯归一化(Gaussian Normalization)，即：

a_i=(a_i-\mu)/\sigma

其中\mu和\sigma分别是每个批次的均值和标准差。

在数据预处理阶段，我们常对输入的原始数据做类似的预处理。BN层把同样的计算过程应用到中间层的输出上。

通过加入BN层，卷积网络可以：

- 增加回传的梯度大小，更好的防止梯度消失的问题；
- 可以使用更大的学习率，而不用过分担心梯度爆炸的问题；
- 降低对于参数初始化的依赖；
- BN层也能起到一定正则化的作用，可以降低网络对于Dropout的依赖。
