nLk-r88-NEE-FbF
# Chapter 4. 模型

卷积网络的结构设计至关重要，不同结构的网络实际效果差别巨大。直观上来说，拥有越深的层次结构、越多节点的卷积网络有着更强的表现力。不论从22层的GoogLeNet，还是2015年ImageNet挑战的冠军具有不可思议的152层的卷积网络现代模型的总体趋势都是朝着越来越深、越来越大的方向发展。但训练这样一个庞大的网络模型并不是件容易的事情，它需要大量的训练数据和计算力，且结构复杂的卷积网络更容易发生过拟合。

在本文中，我们寻求的是一种结构紧凑且有效的网络设计。它应该在能够完成表情识别任务的前提下还能够具备训练简单的优点。为此我们将整个模型分为两个阶段：1）第一阶段包含三个子网络模型，分别用subnet_i,i=1,2,3表示，每个子网络包含数目不等的卷积层；2）第二阶段综合三个子网络模型的输出，得到最终的分类结果。

对于第一阶段而言，三个子网络模型起到提取特征的作用。每个子网络模型的结构，包括卷积层、下采样层的数目以及排列等都不相同。子网络模型的结构差异越大，那么它们的输出的差异也应该越大。通俗来说，不同结构的子网络着重于提取输入图像的不同方面的特征。

第二阶段起的作用时把第一阶段中的三个独立的模型联合在一起。它通过综合不同模型的输出特征来得到最终的结果。这种模型联合的思想与前边提到的Dropout的思想以及决策树发展到随机森零的思想都是相契合的。

在本章节里将详细描述各个子网络的结构，以及整个网络的连接方式，还有它们的训练细节。


## 子网络结构

我们的子网络结构遵循着如下的设计范式：

$INPUT→[[CONV→RELU]×N→POOL?]×M→[FC→RELU]×K→FC$

其中：

- $\times$意味着重复；
- $POOL?$意味着可选；
- 通常 $1\le N \le 3,\  0\le K \lt 3,\  M \ge 0$。

如表1所示，这三个子网络最主要的区别在于卷积层的个数不同。卷积层就如同图像的滤波器，它的目的是从输入中得到有用的特征信息。子网络卷积层个数越多，通常就认为它学习出的特征能够捕获到更多的细节。

下面对于子网络的每一层中的一些细节做如下说明：

**输入层：**现代的图像捕获设备（如手机，相机等）通常都有几百万，甚至几千万的像素分辨率。对于模型训练而言，一方面认为图像的分辨率越高越好，这是因为越高分辨率的图像，细节越丰富；但与之相匹配的模型结构也需要变得复杂，训练时就需要更多的计算力和时间，且更容易发生过拟合的问题。所以输入图像的分辨率又不宜过高。

**卷积层：**我们为所有子网络选择的卷积核大小都为$3\times 3$，这被认为是能够捕捉空间信息的最小的卷积核大小。我们在图像四周加入1个像素的衬垫，并把卷积步长固定为1，这样卷积前后的输入输出图像能够保持分辨率不变。

**非线性层：**我们使用的非线性层激活函数为ReLU函数：$\sigma(x)=max(0,x)$。

**下采样层：**下采样层的窗口大小为$2\times2$，步长固定为$2$的最大值采样，所以并没有采样窗口间并没有重叠。

**全联接层：**在整个网络末端我们堆叠了3个全连接层，应为最重三个模型还要通过全连接层连接在一起，所以这里也可以减少1到2层。

**输出层：**我们选取SoftmaxLoss函数为整个网络的目标函数。用SVMLoss当然也可以，甚至有论文比较了这两个函数的效果，并声称SVMLoss效果更好(提升2%-3%)。

## 整体网络结构

在描述完三个子网络后，就可以开始组建整个卷积网络模型了。如图2描述，整个网络模型包含两个阶段：1）第一个阶段将一张面部表情图片输入到三个卷积子网络中，这三个子网络分别包含8到10层，它们是整个模型最最核心的部分；2）第二个阶段负责根据前一阶段的输出来做出表情的估计。子网络提取到的特征通过一个全连接层连接在一起，最终使用一个SVMLoss层做为整个网络的输出层。

通过这样一个结构，我们就可以把一张表情图片映射到某个具体的表情类别上。整个模型通过综合不同子网络的输出来做出最终的预测。通过综合不同的决策意见来做出更好的判断，这一点也符合我们的直觉常识。每个卷积子网络都可以认为有一定的误判 (而且概率并不低)，在协同工作时，它们就能够形成互补的关系。

## 实现细节

### 开源库

想要从零开始实现一个可用的卷积网络模型可能要耗费一些力气，我们在Github上开源了一个我们用Matlab结合C语言实现的卷积网络模型\cite{Github}。如果想方便且高效的搭建和训练模型，还是推荐使用一些开源的库。成熟的开源库经过精心的设计，且大多具有GPU加速功能，可以大幅度加速耗时的训练过程。

现在流行的深度学习库 (并不仅限于卷积网络) 的实现大致分为两类：命令式的和符号式的。命令式的就如同普通的程序，一步步的求解推倒，比较灵活，能够充分发挥宿主语言的特性。最典型的符号式的库是Torch。符号式的使用类似方程求解的步骤，包含符号的推导，只求解最终的表达式，中间无用的表达式被省略，所以计算起来更加高效。典型的符号式的库包括Theano和Tensorflow。除此之外也有混合两者优点的库比如Mxnet。

本文中我们使用的是MatConvNet。这是一种使用Matlab实现的卷积网络库，专门用于计算机视觉问题。它使用起来非常简单，且支持GPU加速，所以计算也十分高效。

### 实验环境

卷积网络模型的训练对于硬件的要求还是比较高的，特别是对于显卡和内存。训练过程使用GPU加速比不使用GPU加速的时间能缩短3到10倍。在本实验中，我们使用拥有2GB显存的GTX880i显卡搭配16GB内存；软件方面我们在Windows 8.1平台下使用Matlab2015a搭配CUDA6.5加速；MatConvNet版本是当时最新的1.0-beta17。
