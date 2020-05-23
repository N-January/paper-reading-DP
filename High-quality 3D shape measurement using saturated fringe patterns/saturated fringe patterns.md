# N步相移（饱和的正弦光栅）+格雷码

## 参考

High-quality 3D shape measurement using saturated fringe patterns  

## 简述

​	对于高对比度的平面三维测量，可以采用周期为偶数P，步长为P/2的饱和光栅Pattern；或者采用周期为奇数P，步长为P的饱和光栅Pattern，都可以获取好的效果。其中，饱和光栅Pattern可以有效的减缓发光平面的影响。

## 内容

​	内容部分分为基础原理、仿真和实验。

### 基础原理

N步相移法的原理公式为
$$
I_k(x,y)=I^`(x,y)+I^"(x,y)cos[\phi-2k\pi/N]
$$
通过下图的推导，![N步相移推导](E:\学习\项目\标定\High-quality 3D shape measurement using saturated fringe patterns\picture\N步相移推导.jpg)

可得：
$$
\phi(x,y)=-tan^{-1}[\sum_{k=1}^NI_ksin(2k\pi/N)/\sum_{k=1}^NI_kcos(2k\pi/N)]
$$

### 仿真

​	通过上述公式可得：
$$
I_k(i,j)=S*127.5[1+cos(2j\pi/P+2k\pi/N)]
$$
其中S为比例因子，当S>1时，正弦光栅为饱和光栅。则公式可以变成分段函数：
$$
I_k^S(i,j)=\begin{cases}
I_k(i,j) \quad I_k(i,j)\leq255\\
\ 255\qquad  otherwise
\end{cases}
$$
当S为1-6，P=40时，可以得到下图所示的光栅图。

![光栅图](E:\学习\项目\标定\High-quality 3D shape measurement using saturated fringe patterns\picture\光栅图.png)

其中（a）图中S为1，（b）图中S为2，（c）图中S为6.其对应的正弦函数如下所示：

![正弦函数](E:\学习\项目\标定\High-quality 3D shape measurement using saturated fringe patterns\picture\正弦函数.png)

生成标准图后，在将每个图的相位差的均方根误差求出来，得到下图：

![均方根误差](E:\学习\项目\标定\High-quality 3D shape measurement using saturated fringe patterns\picture\均方根误差.png)

从图g、h、i可以得到，饱和的正弦光栅的值更精确。

### 实验

![实验图片](E:\学习\项目\标定\High-quality 3D shape measurement using saturated fringe patterns\picture\实验图片.png)

如图所示为不同S，相同的步长和周期，通过N步相移法和格雷码获取的效果图。

## 结论

该论文的方法适合测量高对比度或高亮度的被测物的三维测量。