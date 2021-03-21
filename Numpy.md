## Morvan tutorial Nnmpy：

1、numpy中array的type叫做dtype，定义初始化：dtype=np.int，表明定义的是整型，输出是整型64位。

![image-20201127095940151](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201127095940151.png)

2、arange可以生成从x到y的范围内的数字，如arange（10，20，2），表示从10开始到20，不包括20，以步长为2递增。arange（12），表示从0开始到11的十二个数字。要想对输出的形状有所要求，就可以用reshape将输出转换为相应的格式。

![image-20201127100919106](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201127100919106.png)

3、axis=0，表示是对行进行操作，axis=1，表示队列进行操作。random是产生随机数，sum、min、max表示分别进行对应的操作。

![image-20201127112200214](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201127112200214.png)