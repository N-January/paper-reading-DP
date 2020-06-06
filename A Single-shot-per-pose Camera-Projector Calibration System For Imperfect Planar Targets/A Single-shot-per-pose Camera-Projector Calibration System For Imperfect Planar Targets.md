这篇论文是关于投影仪标定的；解决了标定时平面畸变造成标定不准，通过标定结构光特征点，减少传播误差，使用one-shot更简单方便；迷人之处：one-shot，使用BA减少畸变误差；新东西：one-shot；为什么神奇：因为之前没人尝试过。

# 一个用于非理想平面标定的Single-shot-per-pose 相机-投影仪标定系统

论文：A Single-shot-per-pose Camera-Projector Calibration System For   mperfect Planar Targets  

# 摘要

本文提出了一种通过单次投射pattern，便可以标定出相机与投影仪之间的内外参数，并且可以通过BA算法去除投影平面的不平整对标定造成的影响。这里的单次投射是指每摆放一次棋盘格投射一次。

# 具体方法

## 内外参标定

### 具体流程

如下图一所示，为本文的实验流程，首先获取采集棋盘格图像，然后投影仪投射De Bruijn 序列结构光，并采集含有结构光（SL）的图像，通过计算该图像上的SL Points是否充足，是便继续进行，否便重新开始。SL points充足的话，便可以确定相机中的SL pettern中的特征点与投影仪中的SL pattern中的特征点之间的关系；并用棋盘格对相机进行标定，求出相机内参和相机与标定板之间的外参，并获得相应的单应性矩阵。通过单应性矩阵和畸变的特征点获取矫正过后的棋盘格的特征点。然后借用双目的标定，标定棋盘格与投影仪之间的特征点，便可以得到相机与投影仪之间的外部参数。然后使用BA算法消除投影平面不平滑造成的误差。

| ![](https://github.com/N-January/paper-reading-DP/blob/master/A%20Single-shot-per-pose%20Camera-Projector%20Calibration%20System%20For%20Imperfect%20Planar%20Targets/picture/procedures.png) |
| :----------------------------------------------------------: |
|                             图一                             |

### 初始标定

#### 相机和投影仪模型

相机和投影仪的内参和畸变参数分别为：

|  相机  | $K_c=\begin{bmatrix} f_x & 0 & c_x\\ 0 & f_y & c_y \\ 0 & 0 & 1 \\ \end{bmatrix}$      内部参数 | $ d_c=\begin{bmatrix} k_1 & k_2 & p_1 & p_2 \\ \end{bmatrix}$ 畸变参数 |
| :----: | :----------------------------------------------------------: | ------------------------------------------------------------ |
| 投影仪 | $K_p=\begin{bmatrix} f_x^\prime & 0 & c_x^\prime\\ 0 & f_y^\prime & c_y^\prime \\ 0 & 0 & 1 \\ \end{bmatrix}$      内部参数 | $ d_p=\begin{bmatrix} k_1^\prime & k_2^\prime & p_1^\prime & p_2^\prime \\ \end{bmatrix}$ 畸变参数 |

相机与投影仪之间的外部参数：

​                                             $r_cp=(r_x,r_y,r_z)^T$         ,         $t_cp=(t_x,t_y,t_z)^T$

#### 相机标定

首先使用张氏标定法标定出相机的内参$K_c$和畸变$d_c$ 包括每次变换棋盘格的相机与棋盘格之间的相对旋转矩阵$R^j_{mc}$和平移矩阵$t^j_{mc}$,并求出每次的单应性矩阵$H^j=K_c*\begin{bmatrix}r1^j_{mc}, & r2^j_{mc}, & t^j_{mc} \end{bmatrix}$,其中$ r1^j_{mc}$和$r2^j_{mc}$是矩阵$R^j_{mc}$的第一列和第二列。

#### 投影仪标定

通过对投射出的pattern做解码，我们可以得到相机拍摄的pattern图案与投影仪中的pattern图案之间的关系，即$\dot{X}^j_m(i)=inv(H^j)*\overline{x}^j_c(i)$,其中$\overline{x}^j_c$是矫正后的单应性矩阵，为了更简便，这里适应的特征点均为SL points不是棋盘格的角点。因为使用SL points更鲁棒更精确。

通过上述，我们可以得到关键点对$(\dot{x}_m,\overline{x}_p)$,然后使用张氏标定法，获取相应的外部参数。

### Bundle Adjustment

暂时没用到，先不做总结。

## 结论

通过看这篇论文的标定方法，有很多可取之处，最重要的还是结合了结构光编解码的能力，使得投影仪的标定更加简单，快捷，并且精确度不会受到相机标定的影响。可以借鉴这里的方法，应用到双目深度获取中。