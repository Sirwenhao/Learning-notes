## Python学习笔记：

1、API: argparse

- python命令行解析的标准模块，内置于python，这个库可以使我们直接在命令行中向程序传入参数并让程序运行
- 使用argparse的第一步即创建解析对象：parser = parser.ArgumentParser()
- parser.add_argument()添加需要的命令行参数和选项
- parser.parse_args()进行解析

2、斋藤康毅的深度学习书：

第一章：绘制图像，pyplot的功能

- ```
  import numpy as np
  import matplotlib.pyplot as plt
  
  x = np.arange(0,6,0.1)
  y1 = np.sin(x)
  y2 = np.cos(x)
  
  plt.plot(x,y1,color = "green",linestyle = ":",label = "sin")
  plt.plot(x,y2,linestyle = "--",label = "cos") #color = "red"可以实现对图形颜色的变化
  plt.xlabel("x")
  plt.ylabel("y")
  plt.title('sin & cos')
  plt.legend()
  plt.show()
  ```

![image-20201211164818818](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201211164818818.png)

- 绘制图像，阶跃函数

  ```
  import numpy as np
  import matplotlib.pyplot as plt
  
  def step_function(x):
      return np.array(x > 0,dtype=np.int)
  
  x = np.arange(-5.0,5.0,dtype = np.int)
  y = step_function(x)
  plt.plot(x,y)
  plt.ylim(-0.1,1.1)
  plt.show()
  ```

![image-20201211164739036](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201211164739036.png)



![image-20201213085800620](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201213085800620.png)

![image-20201213085823990](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201213085823990.png)

![image-20201213090303492](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201213090303492.png)