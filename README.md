## 系统实现方式

如果想从零开始构建一个可用的卷积网络模型，实现所有层的功能可能需要耗费一些时间。我们在GitHub开源了一个我们用Matlab结合C语言实现的一个卷积网络模型。此模型不具备GPU加速功能，所有训练全都在CPU上完成 ，所以训练过程需要耗费较长的时间。

如果想快速的搭建和训练模型，推荐使用一些开源的深度学习库。成熟的开源库经过精心的设计，实现效率高；且大多具有GPU加速功能，可以大幅度加速耗时的训练过程。在本章节我们盘点现在学术界和工业界流行的深度学习库。

### 深度学习库的比较选择

现在流行的深度学习库 (并不仅限于卷积网络) 的实现大致分为两类：命令式的实现和符号式的实现。命令式的就如同普通的程序，一步步的求解推倒，比较灵活，能够充分发挥宿主语言的特性。符号式的使用类似方程求解的步骤，包含符号的推导，只求解最终的表达式，中间无用的表达式被省略，所以计算起来更加高效。

现在学术界和工业界流行的深度学习库包括：

1. Caffe (加州大学伯克利分校)：在工业界被广泛使用，用C++语言实现，计算效率高，且提供Python和Matlab接口。但设计较复杂，需要依赖很多别的功能库。
2. Theano (蒙特利尔理工学院)：符号式实现的代表库，Python实现。缺点是计算效率不高。Theano派伸出很多别的深度学习库如：
   1. Blocks：支持 CNN, RNN 和 LSTM；功能强大，但复杂；
   2. Lasagne：支持 DNN 和 CNN，但不支持 LSTM；功能简单，易于使用。
3. Torch （纽约大学）：命令式实现的代表库，Lua语言实现，计算效率高（LuaJIT效率可跟C语言相媲美），且得到Facebook的支持，发展迅猛。缺点是Lua语言较小众，学习成本稍高。
4. TensorFlow （Google）：谷歌开源的符号式实现库，支持异构设备分布式计算，它能够在各个平台上自动运行模型，从单个CPU / GPU到成百上千个GPU组成的分布式系统。
5. ConvNetJS （斯坦福大学）：JavaScript语言实现的卷积网络库，可以在浏览器上直接运行，是绝佳的效果演示方式，缺点是不支持GPU加速，计算效率低。
6. MXNet（国内DMLC团队）：比较新的深度学习库，C++实现，效率高，综合符号式和命令式两者的优点，且提供众多语言的接口。
7. MatConvNet（vlfeat.org）：使用Matlab实现的卷积网络库，专门用于计算机视觉问题，功能纯粹。使用非常简单，且支持GPU加速，计算高效。

除此之外还有一些库如MSRA开源的DMLC、三星的Veles等等，在此不一一介绍。



### 实验环境

卷积网络模型的训练对于硬件的要求还是比较高的，特别是对于显卡和内存。训练过程使用GPU加速比不使用GPU加速的时间能缩短3到10倍。在本实验中，我们使用拥有2GB显存的GTX880i显卡搭配16GB内存；软件方面我们在Windows 8.1平台下使用Matlab2015a搭配CUDA6.5加速；深度学习库选用的是MatConvNet当时的最新版本1.0-beta17。
