摘要
自动表情识别 (Automated Facial Expression Recognition, 缩写FER) 赋予计算机感知并尝试理解人类情感的能力。在计算机视觉、人机交互和情感计算领域都有着非常重大的研究和应用价值。如果机器能够像人类一样拥有感情的话，那么人类与机器的关系将会被彻底改变，机器对于我们人类而言将不再只是一种工具而已。

表情识别因其独特的情感属性，对于计算机而言存在一定的分析和理解障碍。因为表情是人类内心复杂情感的外在流露，而这些深藏的情感很难被量化、被表达，所以对于计算机而言表情识别并不应简单的归于一种的图像分类问题。

受益于近几年来深度学习的发展，尤其是卷积网络在图像检测和识别方面取得的进展，在本文中我们设计和实现了一种基于卷积网络集成的面部表情识别模型。我们的卷积网络模型包含两个阶段：第一阶段，训练多个子网络模型。这些子网络模型分别包含着个数不同的卷积层，每个卷积层后连接着非线性激活层和池化采样层。这些卷积子网络分别在训练集上训练至收敛。第二阶段，由这些训练好的子网络模型连接在一起构成最终的模型。各个子网络模型通过移除输出层，并把所有倒数第二层的结果连接在一起后输出到若干全连接层中，通过这样的方式使这些单独训练的子网络模型连接成一个整体。通过不同卷积子网络集成的方式，我们充分发挥了不同结构的子模型分类的优势，各个子模型在协同工作中能够发挥互相补充的效果。

​	整个模型输入一张面部表情图片，输出的是七种基本表情的一种，包括｛｝。我们在FER2013\footnote{参考网址：https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge}数据集上训练和测试了整个模型，实验结果显示我们提出的模型的正确率(68.62\%​)非常接近于人类在此数据库上的识别水平(68\pm5 \%​)，且比单个的子模型约有4%的准确度提升。


Automated Facial Expression Recognition (FER) gives machines the ability to read and understand human emotions. It is of vital research and  practical application values in computer vision, human computer interaction, and affective computing. If one day the machines can understand and convey emotions like us, then the relationship will be radically changed, and they will not just be tools to us any more.

There are 7 basic human expression including angry, disgust, fear, happy, sad, surprise and neutral. Besides, there are also many micro-expressions which is very subtle and uncertain. Human expression is the outside voice of inside feelings, so it's very difficult for computers to model this complicated question.

This paper is focusing on the Facial Expression Recognition (FER) problem from a single face image. Inspired by the advances Convolutional Neural Networks (CNNs) have achieved in image recognition and classification, we propose a CNN-based approach to address this problem. Specifically, our network consists of several different structured subnets. Each subnet is a compact CNN,  which contains different number of convolution layers followed by non-linearity and pooling layers. The whole network is structured by assembling these trained subnets together. 

It takes face images as input and classifies them into one of the seven basic human expressions. Our model is trained and evaluated on the FER2013 dataset\footnote{参考网址：https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge}. The experiment results show that the proposed model achieves similar results to the human level(68\pm5\%​) on this dataset.
