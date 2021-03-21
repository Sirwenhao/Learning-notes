# Pytorch Tutorial：

### Part 1：PyTorch and Tensors

![image-20201203171431529](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201203171431529.png)

![image-20201203171444583](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201203171444583.png)

### Part 2：Neural  Networks  and  Deep  Learning  with  PyTorch

![image-20201203171747982](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201203171747982.png)

1、Soumith funder  of PyTorch

2、torch.nn 代表神经网络，包含类和模块。比如图层、权重和前向函数。

3、torch.autograd 是一个子包，负责处理在核心处优化我们的神经网络权重所需的导数计算。

4、所有的深度学习框架都有两个特性：一个张量库和一个用于计算倒数的包。在PyTroch中就是torch.nn和torch.autograd。

5、torch.nn.functional是功能接口，能够让我们访问像损失函数，激活函数和卷积运算。torch.optim是我们能够访问典型的优化。

6、torch.utils是一个子包，它包含像数据集和数据加载器这样的实用程序类，是数据处理更加容易。