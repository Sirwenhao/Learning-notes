## Attention Module

#### 1、什么是注意力机制？计算机视觉（computer vision）中的注意力机制（attention）的基本思想就是想让系统学会注意力——能够忽略无关信息而关注重点信息。

#### 2、当前注意力机制的主要分类（按照关注的区域来划分）：

- 空间域（spatial domain）

- 通道域（channel domain）

- 层域（layer domain）

- 混合域（mixed domain）

- 时间域（time domain）

- 注意力域（attention domain）：使用强化学习（reinforcement learning），训练与上述的几个有所不同

  ##### 2.1、通道注意力机制和空间注意力机制

  2.1.1、通道注意力机制：Convolutional Block Attention Module (CBAM) 表示卷积模块的注意力机制模块。是一种结合了空间（spatial）和通道（channel）的注意力机制模块。

  论文地址：[CBAM: Convolutional Block Attention Module](https://link.zhihu.com/?target=http%3A//openaccess.thecvf.com/content_ECCV_2018/papers/Sanghyun_Woo_Convolutional_Block_Attention_ECCV_2018_paper.pdf)

  CBAM算法：主要创新点在于提出了一种轻量、通用的attention module，同时在==空间和通道上==进行特征的attention，这样不只能够==节约参数和计算力==，并且保证了其能够做为==即插即用==的模块集成到现有的网络架构中去。

  ![preview](https://pic3.zhimg.com/v2-54172da177c50ff424194548518a14fa_r.jpg)

  CBAM包括两个模块，分别进行通道上和空间上的attention，如图：

  

  ![img](https://img-blog.csdnimg.cn/20210310210545901.png#pic_center)

  ![img](https://img-blog.csdnimg.cn/20210310212315596.png#pic_center)

  具体流程如下：
         将输入的*特征图F（H×W×C）*分别经过==基于width和height==的*global max pooling（全局最大池化）*和*global average pooling（全局平均池化）*，得到两个==1×1×C的特征图==，接着，再将它们分别送入一个==两层的神经网络（MLP）==，第一层神经元个数为 C/r（r为减少率），激活函数为 *ReLu*，第二层神经元个数为 C，这个两层的神经网络是共享的。而后，将MLP输出的特征进行==基于element-wise的加和==操作，再经过==sigmoid激活==操作，生成最终的channel attention feature，两个attention模块最后也都是使用*sigmoid*来缩放到[0,1]之间，即M_c。最后，将M_c和输入特征图F做*element-wise乘法*操作，生成Spatial attention模块需要的输入特征。

  - 为什么要使用池化？

    ![img](https://img-blog.csdnimg.cn/20210310220204142.png#pic_center)

    在channel attention中，表1对于pooling的使用进行了实验对比，发现avg & max的并行池化的效果要更好。*这里也有可能是池化丢失的信息太多，avg&max的并行连接方式比单一的池化丢失的信息更少，所以效果会更好一点。*【Giga F**l**oating-point Operations Per Second,即每秒10亿次的浮点运算数,常作为GPU性能参数的参考指标】

    2.1.2、空间上的Attention Module

    ![img](https://img-blog.csdnimg.cn/20210310210627977.png#pic_center)

    ![img](https://img-blog.csdnimg.cn/20210310213341909.png#pic_center)

    具体流程如下：
           将==Channel attention模块输出的特征图F‘==作为本模块的输入特征图。首先做一个==基于channel的global max pooling 和global average pooling==，得到两个==H×W×1==的特征图，然后将这2个特征图==基于channel 做concat操作（通道拼接）==。然后经过一个==7×7卷积==（7×7比3×3效果要好）操作，==降维为1个channel，即H×W×1==。再经过==sigmoid==生成spatial attention feature，即M_s。最后将该==feature和该模块的输入feature做乘法==，得到最终生成的特征。

    2.1.3、通道注意力模型（CAM）和空间注意力模型（SAM）的组合

     通道注意力和空间注意力这两个模块能够以==并行或者串行==顺序的方式组合在一块儿，关于通道和空间上的串行顺序和并行作者进行了实验对比，发现==先通道再空间==的结果会稍微好一点。具体实验结果如下：

    ![img](https://img-blog.csdnimg.cn/20210310220552369.png#pic_center)

  

  

  

