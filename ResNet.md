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

1、为什么网络层数越深，误差越大？在ResNet中作者提出了两个问题：

- 随着网络层数的加深，梯度消失和梯度爆炸以及梯度爆炸这个问题会越来越明显（假设在反向传播过程中，每一层的误差梯度是一个小于一的数，在反向传播过程中，每向前传播一次都乘以一个小于一的系数，那么当网络越来越深的时候所乘的小于一的系数就越来越多，梯度就会越来越趋近于零，造成梯度消失。相反，再反向传播过程中，如果每一层的梯度都是一个大于一的数，则在反向传播过程中梯度会越来越大，即梯度爆炸）。梯度消失和梯度爆炸问题的解决通常是通过：对数据进行标准化处理、权重初始化以及Batch Normalization（放弃dropout）来解决。
- Degredation problem：即在解决了梯度消失和爆炸的问题之后，依然会存在网络层数加深，但是效果依然不好的问题。ResNet提出了残差结构，通过残差结构，解决退化问题。