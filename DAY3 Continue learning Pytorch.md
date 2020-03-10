关于NDArray的一些代码
====基础====
from mxnet import nd
x = nd.arange(12)  %0~11
x.shape  %(12,0)
x.size  %12
X = x.reshape((3, 4))  %注意.reshape()方法里面还有一个(3,4)，默认先填满一行
    %x.reshape((3,-1))中的-1表示会根据元素个数自动调整。但是这里有一个问题，如果不是整除关系，调整后的总元素数始终比原本的少（即-1是整除向下取整）
nd.zeros((2, 3, 4))  %方法为.zeros()
nd.ones((3, 4))      %方法为.ones()
Y = nd.array([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])  %因为矩阵本身的表示就是[[],[],[]]，所以在.array()方法里也要是[[],[],[]]
nd.random.normal(0, 1, shape=(3, 4))  %第一个是均值，第二个是方差
Z=Y.zeros_like() 创建与Y形状相同但是都是0的Z

====运算====
X + Y  X * Y  X / Y  Y.exp()  X==Y%按元素加减乘除指数变化

nd.dot(X, Y.T)  %点乘

nd.concat(X, Y, dim=0)  %上下连接
nd.concat(X, Y, dim=1)  %左右连接

X.sum().asscalar()  %变成标量

范数(norm)是数学中的一种基本概念。它常常被用来度量某个向量空间中的每个向量的长度或矩阵变化的大小。
X.norm()  %求范数

这样表达也是对的：nd.exp(Y)、nd.sum(X)、nd.norm(X)

====广播机制====
[[0]
 [1]       +      [[0 1]]
 [2]]
变成
[[0  0]               [[0  1]
 [1  1]       +        [0  1]
 [2  2]]               [0  1]]

====索引====
第一个开始标的是0
X[1:3] 表示：标号为[1,3)行，即第二行和第三行
X[1,2] 表示第二行第三列的元素
X[1, :]=12  把标号为1的行全部标成12
X[1:3, :]=12  把标号为1和2的行全部标成12

====内存相关====
1.  nd.elemwise_add(X, Y, out=Z)   %防止X和Y在相加过程中产生多余的内存开销
2.  X[:] = X + Y
3.  X += Y

====与numpy互相转换====
nd.array()：将numpy→NDarray
.asnumpy()：将NDarray→numpy

====自动求梯度====
举例：对于
![text](https://github.com/Milomilo0320/Python-Opencv-Learning-Diary/blob/master/picture/1.gif)
from mxnet import autograd, nd

