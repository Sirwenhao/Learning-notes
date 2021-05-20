## SE_ResNet的pytorch实现

#### 残差块：

```text
class Resiual_block(nn.Module):
    def __init__(self, in, middle_out, out, kernel_size=3, padding=1):
        self.out_channel = middle_out
        super(Resiual_block, self).__init__()

        self.shortcut = nn.Sequential(
            nn.Conv2d(nin, nout, kernel_size=1),
            nn.BatchNorm2d(nout)
        )
        self.block = nn.Sequential(
                                nn.Conv2d(in, middle_out, kernel_size=kernel_size, padding=padding),
                                nn.BatchNorm2d(middle_out),
                                nn.ReLU(inplace=True),
                                nn.Conv2d(middle_out, out, kernel_size=kernel_size, padding=padding),
                                nn.BatchNorm2d(nout))


    def forward(self, input):
        x = self.block(input)
        return nn.ReLU(inplace=True)(x + self.shortcut(input))
```

**注意** 在input与输出相加前，这里需要一个shortcut层来**调整input的通道大小**

#### SE-Net：

```text
class SE(nn.Module):
    def __init__(self, in, middle_out, out, reduce=16):
        super(SE, self).__init__()
        self.block = Resiual_block(in, middle_out, out)

        self.shortcut = nn.Sequential(
            nn.Conv2d(nin, nout, kernel_size=1),
            nn.BatchNorm2d(nout)
        )
        self.se = nn.Sequential(
                                nn.Linear(out, out // reduce),
                                nn.ReLU(inplace=True),
                                nn.Linear(out // reduce, out),
                                nn.Sigmoid())


    def forward(self, input):
        x = self.block(input)
        batch_size, channel, _, _ = x.size()
        y = nn.AvgPool2d(x.size()[2])(x)
        y = y.view(y.shape[0],-1)
        y = self.se(y).view(batch_size, channel, 1, 1)
        y = x * y.expand_as(x)
        out = y + self.shortcut(input)
        return out
```

**注意** 我们使用平均池化层进行下采样，**这里的kernel_size是动态的**

## ResNet

#### 1、为什么网络层数越深，误差越大？在ResNet中作者提出了两个问题：

- 随着网络层数的加深，梯度消失和梯度爆炸以及梯度爆炸这个问题会越来越明显（假设在反向传播过程中，每一层的误差梯度是一个小于一的数，在反向传播过程中，每向前传播一次都乘以一个小于一的系数，那么当网络越来越深的时候所乘的小于一的系数就越来越多，梯度就会越来越趋近于零，造成梯度消失。相反，再反向传播过程中，如果每一层的梯度都是一个大于一的数，则在反向传播过程中梯度会越来越大，即梯度爆炸）。梯度消失和梯度爆炸问题的解决通常是通过以下三种方式解决：
  	    1. 对数据进行标准化处理
     	    2. 权重初始化
     	    3. Batch Normalization（放弃dropout）
- Degredation problem：即在解决了梯度消失和爆炸的问题之后，依然会存在网络层数加深，但是效果依然不好的问题。ResNet提出了残差结构，通过残差结构，解决退化问题。

#### 2、ResNet网络中残差块的结构：

![image-20210520081354288](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520081354288.png)

#### 3、ResNet原论文中作者遇到的问题：

1、通过简单堆叠卷积层与池化层而形成的深层网络存在：层数越深，网络效果越差的问题。作者提出了1处的两个问题。关于梯度消失或者梯度爆炸的本质原因：网络太深，网络权值更新不稳定，本质是因为梯度反向传播过程中的连乘效应（链式法则）。关于梯度消失和梯度爆炸的直观解释：

- 以三个隐藏层为例：

  $$y_i=\delta(z_i)=\delta(w_ix_i+b_i)$$,$$C$$为代价函数，根据链式法则，可以得到图中表达式

  ![image-20210520082810809](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520082810809.png)

- Sigmoid函数的导函数图像为：导函数最大值为$$1/4$$

![image-20210520091308510](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520091308510.png)

![image-20210520091437158](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520091437158.png)

#### 4、ResNet残差结构：

- 主要分为两种：针对相对浅层的网络如ResNet18，ResNet34和针对相对深层的网络ResNet50、ResNet101以及ResNet152等
- 残差结构①：

![image-20210520091844823](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520091844823.png)

​		此处参数量计算的解释：两个256是指，假设输入特征矩阵的维度是256，主线上卷积核的个数也得对应是256.

- 残差结构②：此种结构主要是针对ResNet50、Resnet101、ResNet152等深度网络使用的。

  ![image-20210520100702628](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520100702628.png)

![image-20210520100722057](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520100722057.png)

​	此处参数计算量解释：输入特征图的维度是256，但第一个卷积层的卷积核的数量是64

#### 5、ResNet论文中的结构图：

![image-20210520101515695](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520101515695.png)

![image-20210520102944933](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520102944933.png)

#### 6、以ResNet34为例：

![image-20210520103714054](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520103714054.png)

​	注意：Conv3_x、Conv4_x、Conv5_x各个部分之间相连接的第一个部分均为optionB这种残差结构，因为通道数不匹配，但Conv2_x处直接使用第一种结构，因为在这一层输入与输出的通道数是相同。

#### 7、ResNet50、ResNet101、ResNet152基础结构：

![image-20210520104757607](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520104757607.png)

 - 虚线的残差结构键输入图片的高和宽以及深度等都做变化，而实线部分的输入特征矩阵的和输出特征矩阵是一样的。
 - Conv3_x、Conv4_x、Conv5_x对应的一系列残差结构的第一层均为虚线结构，因为第一层必须将上一层的特征矩阵的高和宽以及深度调整为当前层特征矩阵所需的高和宽以及深度（原文中：Downsampling is performed by Conv3_1，Conv4_1，Conv5_1 with a stride of 2）.

### Batch Normalization

1、Batch Normalization的目的是将一批（Batch）feature map满足均值为0，方差为1的分布规律，即将一批数据对应的feature map（即特征矩阵）的每一个维度也就是每一个channel对应的维度满足均值为0，方差为一的分布规律。

2、通常情况下，在将图像输入到网络之前，会对图像进行预处理。即将输入图像数据调整到满足某一分布规律，这样可以加速网络训练。但原始图像在通过卷积层之后，得到的feature map却不一定满足某一分布规律，因此考虑使用Batch Normalization调整feature map，使每一层的feature map都能满足均值为0和方差为1的分布。

3、

![image-20210520105914662](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520105914662.png)

![image-20210520105937512](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20210520105937512.png)

​	注：此处 $$\beta$$ 和 $$\gamma$$ 的初始值分别为0和1.

