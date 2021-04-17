## 生成对抗网络（GAN）的算法原理

### 生成器Generator G

### 判别器Discriminator D

### 原始真实数据集Database

### Algorithm

step1：固定生成器G，更新判别器D。在此出生成器的原始输入是来自于随机的抽样，原始的抽样是来自于真实的样本数据集。此处，生成器所生成的数据类型一定是与原始数据类似的。

![image-20201029133601345](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029133601345.png)

step2：固定判别器D，不断更新生成器G。原因：生成器的目的在于产生非常接近于原始样本数据集的结果，并确保结果在输入进判别器时能够得到高的分数，即“骗过”判别器。在代码实现部分，会将生成器与判别器合起来当作一个large Network。中间层的hidden layer会是比如说64*64维的矩阵，可以看作是一个灰度图。而之所以固定后面的hidden layer（即判别器）的原因就是防止在最后输出的网络结构中权重因子过大而导致最终的输出结果评分很高，但实际上的值并不符合。

![image-20201029134505020](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029134505020.png)

数学原理:两部分反复执行

![image-20201029140717317](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029140717317.png)

GAN：Structured Learning Approach

![image-20201029142043820](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029142043820.png)

如何使输入图像被转换为一系列标签值，使用自动编码器AutoEncoder；使标签值转化为图像，使用解码器Decoder

![image-20201029142814507](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029142814507.png)

生成器和判别器优缺点对比：

![image-20201029153903373](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201029153903373.png)

生成对抗网络的数学原理部分：









Conditional GAN algorithm：

![image-20201103094039121](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103094039121.png)

- 输入都是文字和图像组成的输入样本对，文字为c，图像为x。
- 采样m个噪声样本
- 通过将文本和噪声样本组成的样本对输入到生成器网络中，形成m个生成的样本x-hat
- 从样本数据集中抽样出m个样本x-^
- 相比与原始的GAN的损失函数，Conditional GAN中增加了一种negative example，损失函数中最后一项，表示的是从样本数据集中提取出的样本，但是跟c不配对
- 第二个表示文本c和生成出的模糊图像配对的损失



1、监督学习下的文本-图像生成

![image-20201103085946352](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103085946352.png)

存在的问题：给定文本之后，根据文本生成的图像可能有很多种，但并不是每种都合适

2、Conditional GAN中存在的问题

![image-20201103090516281](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103090516281.png)

在Conditional GAN中生成器部分同时输入文本序列和从正态分布中取样出的Z同时输入到生成器中，根据这段输入的文字和去养出的vector同时产生的image

![image-20201103093258620](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103093258620.png)

在Conditional GAN中判别器部分也需要同时喂入两个输入，并且输出的scalar用于衡量两个输出：第一个是判断x是否是真实的，第二个判断c和x是否是真的match。两种不匹配的情况：文本对，图像不对；文本不读，图像对。这两种情况都要判零。

3、两种常用的判别器网络结构设计：

![image-20201103144409034](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103144409034.png)

- 两者在效果上差别不大，其输出都可以衡量判别器所做的两件事情：1.判断x是真是假，2.判断c和x是否match
- 但是在效果上两者的差别在于：第一种网络设计的输出只有一个分数，当输出值较低时无法判断究竟是因为x的真实度不够还是c和x的匹配程度不够造成的低输出
- 第二种结构可以很详细的分辨输出部分中低分数的具体原因（蓝色字体的这三篇论文有讲此种网络设计相关的内容）

4、Stack GAN：

![image-20201103145857435](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201103145857435.png)

- stack GAN的提出是为了解决Conditional GAN无法生成高清大图
- stack GAN最终产生的是256乘256的清晰大图，核心思想是搭建两个gengrator。第一个产生64*64的小图，然后把第一个generator的结果放入到第二个generator中产生256乘256的大图
- 第一个阶段：是一个标准的Conditional GAN，其输入为标准正态分布的采样z和文本描述刻画的向量c，第一步通过对抗生成网络生成一个低分辨率的64乘64的图片和真实数据经过对抗训练得到的粗粒度生成模型
- 第二阶段：Stack GAN将第一阶段的生成结果和文本描述作为输入，用第二个生成对抗网络生成高分辨率的256乘256图片

5、无监督条件生成器：

![image-20201104142218054](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104142218054.png)

- 第一种方法：直接转换，但是输入输出差别不大，适用于纹理 以及 颜色变换
- 第二种方法：使用与input 与 output差别较大。如上图编码器抽取人脸特征，解码器通过抽取到的特征生成图像

第一种方法：

![image-20201104142957577](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104142957577.png)



x输入到生成器G后产生类似于y中的图像，判别器D是在y的domain中训练出的，所以D可以判别图像是否属于domain Y

![image-20201104143339723](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104143339723.png)

但存在的问题是，为了使迷惑判别器D，生成器G有可能生成与x无关但是与y domain高度相似的图片，导致判别器D出现误判。所以我们的目的不仅仅是让生成器G产生的图像可以成功迷惑判别器D，还必须确保输入输出有一定程度上的关联。The issue can be avoided by network design

![image-20201104151051198](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104151051198.png)

另一种方式：将生成器G产生的图像直接给判别器D，但同时要求原始图像和生成图像在经过预训练的编码网络中的输出使尽可能相似的。其实也是要求输入输出尽可能相似，核心思想相同。

![image-20201104151903126](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104151903126.png)

第三种方式：其实就是cycle GAN，训练两个生成器x--y和y--x的。如上图，经过两次生成器变换之后（x domain to y domain and y domain to x domain）的图像要和原始图像尽可能相似，其核心思想依旧是限制生成器G的输入输出，使其尽可能相似。经过两次生成器变化之后可以转换回原始图像成为Cycle consistency

![image-20201104152217730](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104152217730.png)

Cycle GAN的结构即为上图，一个方向从x domain 映射到y domain，另一个方向从y domain 映射到x domain。两条线，同时训练。

第二种方法：

![image-20201104163632867](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104163632867.png)

x domain的编码器提取x domain的真实人脸图像的特征，这种特征是一个向量，有很多标签值分别代表不同的特征，y domain的编码器提取y domain的二次元人脸图像的特征。两个编码器的network不一定相同，由于其训练的训练集是不同的。上述结构希望达成的效果是：当输入一张x domain的人脸图像时，提取的特征在输入到解码器DEx后其输出就是原来的输入人脸图像，输入到解码器DEy之后输出的即为二次元任务图像。

![image-20201104170730939](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104170730939.png)

在训练时，两组编码器和解码器都需要minimize reconstruction error。但是此种方法存在的问题是这些enconder和decoder之间没有任何关联。



![image-20201104171207483](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104171207483.png)

如果只关注minimizing reconstruction error时，decoder输出的图像会很模糊，所以在image后加上一个各自domain 的 decoder，这个decoder会鉴别所输出的image是否为相应domain的图，以此强迫DEx，DEy使其输出的image都比较真实。以上编码器ENx、解码器DEx和判别器Dx一起组成VAE GAN（如下图）。

![image-20201104171810142](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104171810142.png)



## Theory  behind  GAN：

#### Generation：Using Generative Adversial Network(GAN)：

![image-20201104221848415](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104221848415.png)

目的：让机器找出distribution----Pdata(x)，在蓝色区域内生成高质量的image，在蓝色区域外生成低质量的image的这种分布。

![image-20201104223302356](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201104223302356.png)

用极大似然估计去拟合分布Pdata(x)，通过调整$ \ P_G(x;\theta)$的方差和均值使其不断缩小与分布$/P_{{data}} (x)$的差距。注：Typora中输入公式下标，需要用{{}}来限定范围，上标用上三角，下表用下划线。有关Typora的符号使用都放在了Google浏览器中的学习文件夹中。

![image-20201105102205502](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201105102205502.png)

极大似然估计与极小KL Divergence是相同的。此处，$E_x\sim_{P_{data}}$代表积分运算$\int_xP_{data}(x)*\star dx$运算。其中倒数第二行中添加一个被减项，其实就是为了配成KL Divergence的形式，此被减项并不会影响到参数 $\theta$ 。（查下KL Divergence）

![image-20201105170028601](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201105170028601.png)

此部分的生成器G的理想输出就是找到一个输出$P_G(x)$，并保证此输出与目标分布$P_{data}(x)$越接近越好。目的是最小化divergence，但是问题是$P_G$和$P_{data}$是不知道的，而关键就在于此。而GAN的奇妙之处也就在于此。

![image-20201106091938201](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106091938201.png)

由于$P_G$ 和 $P_{data}$ 的具体分布的表达式是不知道的，两者的divergence是通过从Database抽样出的样本$P_{data}$ 和 随机抽样出的样本在经过生成器G生成样本$P_G$ 以此来衡量两者之间的divergence。这也是GAN神奇的地方，通过discriminator可以衡量两者之间的divergence。

![](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106094539593.png)

在训练Discriminator时，要使Generator  fixed。然后最大化此时的目标函数$V(G,D)$ ，因此需要最大化其中的两项，所以当x是从$P_{data}$ 中sample出的话就需要使$D(x)$ 越大越好，即第一项越大越好；当x是从$P_G$ 中sample出来的话，就需要使$D(x)$ 越小越好。其实此表达式与二分类问题的是相同的，此Discrimator所做的事情其实和二分类问题相同。

![image-20201106094918679](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106094918679.png)

两种不同的divergence在训练判别器时的不同情况，如果原始样本差异较大，判别器比较容易判别；如果原始样本差异较小，判别器识别相对较难。

寻找Discriminator的数学推导过程：

![image-20201106095754670](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106095754670.png)

![](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106095938288.png)

![image-20201106100217364](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106100217364.png)

![image-20201106100441360](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106100441360.png)

![image-20201106101409765](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106101409765.png)

![image-20201106101430002](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106101430002.png)

$P_G$ 和 $P_{data}$ 之间的divergence即为$\max_DV(D,G)$ 。

![image-20201106155037888](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106155037888.png)

实际上判别器D的训练就是一个二分类器问题，并且输出是sigmoid函数，范围为0~1。最终Minimize Cross-entropy等同于Maximize目标函数。

![image-20201106160238878](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106160238878.png)

### GAN的算法总结：

- 判别器D的目的是最大化目标函数V，相当于Train一个binary classification问题，缩小其交叉熵损失函数就相当于最大化目标函数。
- train判别器Discriminator的目的是评估JS Divergence，只有当V的值最大的时候，Discriminator才可以评估JS Divergence。为了让V取最大值，所以需要Train很多次，直到收敛为止。但实际上没有办法真正找到一个Discriminator真正使*V(G,D)*取最大值，一般找到的只是一个lower bound。
- Train生成器G时，V中的第一项与生成器G是无关的（第一项都是来源于判别器的样本），因此只需要最小化第二项。
- 生成器G不能update很多次，否则Discriminator就无法评估JS Divergence。

![image-20201106162723456](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106162723456.png)

Ian Goodfellow论文中在实际实施中所选择的函数，即图中所示的两条曲线，曲线的大概变化趋势相似，但是在某一位置上的具体斜率不同

![image-20201106163648826](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106163648826.png)

GAN的直观理解

### Tips for improving GAN：

- 传统方法的衡量指标JS  Divergence的缺陷：

![image-20201106193718452](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106193718452.png)

即使$P_G$ 与 $P_{data}$ 在不断接近，但只要没有重叠区域，JS Divergence的表达式都是一样的，此时此衡量指标失效。

![image-20201106194748994](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106194748994.png)

WGAN，使用EM Distance代替KL Divergence衡量$P_G$ 和 $P_{data}$ 之间的距离。

![image-20201106195011822](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106195011822.png)

P变换到Q可以有很多种变化方法，WGAN的做法就是穷举所有的方法，并将这些方法中平均距离最小的那个定义为earth mover‘s distance。上述所举例子以土作为类比，那么P到Q的变化可以看作如何移动土堆是P和Q相同，所有的移动方案中的平均距离最小的那个方案的距离就是Earth  Mover's  Distance。

![image-20201106200033919](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106200033919.png)

所谓的移动方案就是一个矩阵，矩阵中的值就表示所移动的土的量。

![image-20201106202144062](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201106202144062.png)

必须保证判别器D足够平滑，这样不会在生成部分无限小而真实样本部分无限大，导致V(G,D)的数值可以很大，但实际上效果并不好。

![image-20201107083233118](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107083233118.png)

由原始的GAN转变到WGAN的算法改变：将目标函数中原来的对数函数部分替换，输出将不再是sigmoid函数而是一个线性函数，这样最终的距离才是wasserstein distance。

![image-20201107083638192](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107083638192.png)

接着，在训练 $\theta_d$ 时 必须加入Weigth clipping或者Gradient Penalty，否则不会收敛。

![image-20201107083857231](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107083857231.png)

生成器部分，将原来的对数函数部分改成![image-20201107084002547](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107084002547.png)。

综上所述：在从原始的GAN到Wasserstein GAN的变化过程中，有四处变化：

- 第一部分：![image-20201107084139880](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107084139880.png)，目标函数的变化。
- ![image-20201107084201373](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107084201373.png)，将判别器的输出从sigmoid变换成线性。
- ![image-20201107084245870](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107084245870.png)，加入权重裁剪或者梯度惩罚。
- ![image-20201107084327028](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107084327028.png)，生成器部分目标函数改掉原来的对数形式。



### Feature Extraction：

![image-20201107090959446](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201107090959446.png)

VAE-GAN就是VAE变分自动编码器和生成对抗网络GAN的结合。VAE包含encoder和decoder两个部分，GAN包括decoder（将其作为GAN的生成器）和Discriminator两个部分，其中共享Decoder部分。





























### 补充：受限玻尔兹曼机

首先玻尔兹曼机（Restricted Boltzmann Machines，RBM）的定义：是一类具有两层结构、**对称连接且无自反馈**的随机神经网络模型，**层间全连接，层内无连接**。

![img](https://img-blog.csdnimg.cn/20181120084329236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjM5ODY1OA==,size_16,color_FFFFFF,t_70)