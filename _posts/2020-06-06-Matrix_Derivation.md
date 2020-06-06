---
layout: post
title:  "AI Study Notes 8 - 矩阵求导"
date:   2020-06-06 08:50:18 +0800
categories: AI
tags: AI Machinelearning math
author: Hank.S
---

**关键词**  
*matrix derivation*  





---
* content
{:toc}

### 0. 前言
首先来看看普通人的困惑在哪，于是 Caidu 了一下，看到了 BSDN 上的矩阵求导教程（引用自：https://blog.csdn.net/daaikuaichuan/article/details/80620518）：
![在这里插入图片描述](https://images.gitbook.cn/3c5affc0-0756-11ea-aeb3-051476df08cc)
魔幻的截图质量（来自维基），粗犷的标注，基本没有人看懂的解释，这个纯正的味道有种想让人口吐莲花的感觉。想想深度学习要记住这么多公式都有点头大，这是不可能的完成的任务，反正我是不行也不相信有人可以。由此可见，有些人不是学不会，而是被扔坑里就没上来过。
### 1. 矩阵运算符号系统
#### 1.1 矩阵和运算的分量表示
我们在写矩阵的时候通常会这样写：
$$W$$
这是表示矩阵的形式，当然推荐大家使用分量的形式表示即（迫不得已用的上标）：
$$w_{ij}$$
看起来顺眼多了。在来看看矩阵乘法如何表示的：
$$Y=X\cdot W \rightarrow y_{ij}=\sum x_{ik}\cdot w_{kj}$$
上式对 $k$ 求和就是一个矩阵的乘法了，可以称为点乘。加法符号看着别扭直接去掉：
$$
y_{ij}=x_{ik}\cdot w_{kj}
$$
这便是约定求和，用在张量分析里挺好用的。写点公式不要太方便，同样的也可以用在机器学习的矩阵求导之中。
#### 1.2 补充两个算子
在开始求导前来介绍两个简单的算子： $\delta_{ij}$ 和 $\varepsilon_{ijk}$ 。这两个符号代表不同的数学含义，对于 $\delta_{ij}$ 仅有 $ij$ 相等时等于 1 ，其他情况等于 0 。这是单位矩阵 $I$ 的分量表示。举个例子：
$$A=I\cdot A\rightarrow a_{ij}=\delta_{ik}\cdot a_{kj}$$
实际上其仅起到指标替换的作用。 $\varepsilon_{ijk}$ 当 $ijk$ 有相等的数字时为0，为奇排列时为1，偶排列时为-1。这是向量的叉乘：
$$C = A\times B\rightarrow c_i = \varepsilon_{ijk} a_j b_{k}$$
其还有着矩阵行列式的用处：
$$|A|=\varepsilon_{ijk}a_{1i}a_{2j}a_{3k}$$
别忘了相同指标代表求和。看不懂么有关系，**这个符号我们应该会用得比较少**。

### 2. 该上导数了
#### 2.1 从一个矩阵和向量的乘法开始
矩阵乘法可以表示为：
$$Y=AX$$
写成熟悉的分量形式：
$$y_{ij}=a_{ik}x_{kj}$$
好的，接下来就是求 $\frac{\partial y}{\partial x}$ 了，很简单：
$$
\frac{\partial (a_{ik}x_{kj})}{\partial x_{kj}} = a_{ik}\rightarrow A
$$
这是非常轻松就可以完成的计算。**完全不用死记硬背**。 来个 BSDN 那个公式看起来非常难记的：
$$
\frac{\partial X^\top A}{\partial X}=A^\top
$$
来分量形式
$$
y_{ij}=x_{ik}a_{kj}
$$
偏导数来了：
$$
\frac{\partial (x_{ik}a_{kj})}{\partial x_{ki}}
$$
似乎卡主了，因为 $x_{ik}$ 和 $x_{ki}$ 似乎不相同，无法消去了。实际上可以将指标替换一下，即可：
$$
\frac{\partial (x_{ki}a_{jk})}{\partial x_{ki}} = a_{jk}\rightarrow A^\top  
$$

#### 2.2 损失函数其实不好理解
记住那些公式没用的一点在于实际的情况可能更加复杂。比如损失函数 $L$ 喜欢这样写：
$$
L = \textrm{sum}\left(\left(Y-D\right)_2\right)
$$
损失函数 $L$ 需要对 $Y$ 求导。有一句话，当我们不知道怎么做的时候实际上是我们对任务拆分的还不够细。实际上上面的公式我们应当拆分为一个减法，一个乘法操作。像这样：
$$
A = Y - D \\
B = A \odot A \\
L = \textrm{sum}(B)
$$
求和的确不好做，但是别忘了，求和中矩阵中每个元素是独立的，因此当求和结果对于每个参数进行求导后，得到的是元素全为 1 的矩阵。如果这个不懂，我们来使用一个矩阵的思想。假设两个向量 $pq$ 中元素全为 1 。此时计算方式为：
$$
l_{ij}=p_i b_{ij} q_j
$$
求导
$$
\frac{\partial l_{ij}}{\partial b_{ij}}=p_i q_j = 1
$$
B对A求导为：
$$
\frac{\partial a_{ij}a_{ij}}{\partial a_{ij}}=2 a_{ij}
$$
$A$ 对 $Y$ 求导全为 1，这里就不写了，有兴趣的可以自行推演。由此我们发现了这个损失函数对于 $Y$ 的偏导数就是：
$$
\frac{\partial L}{\partial Y}=2 (y-d)_{ij}
$$

#### 2.3 加上一个模型怎么办
这里我们定义的模型是
$$
Y = X\cdot W + b
$$
损失函数就是 2.1 节所说的。求导怎么办？一步一步来。
首先需要求的是 $L$ 对于 $W$ 的导数。首先写出分量形式：
$$
y_{ij}=x_{ik}w_{kj}+b_j
$$
对 $w$ 的导数写出来：
$$
\frac{\partial L}{\partial Y}\frac{\partial Y}{\partial W}=2(y-d)_{ij}\frac{\partial (x_{ik}w_{kj})}{\partial w_{kj}}=2(y-d)_{ij}x_{ik}=2((Y-D)^\top X)_{\top}
$$
这里看到因为最终计算结果中指标 $y_{ij}$ 与 $x_{ik}$ 是对相同指标 $i$ 进行的求和，因此需要加上一个转置。
**如果想求对于 X 的导数怎么办？**，也好说：
$$
\frac{\partial L}{\partial Y}\frac{\partial Y}{\partial X}=2(y-d)_{ij}\frac{\partial (x_{ik}w_{kj})}{\partial x_{ik}}=2(y-d)_{ij}w_{kj}=2(Y-D) W^\top
$$
这就无需多解释了。还有一个小的问题，$b$ 别忘了。这个就更简单了
$$
\frac{\partial L}{\partial Y}\frac{\partial Y}{\partial b}=2(y-d)_{ij}\frac{\partial (x_{ik}w_{kj}+1_{i}b_j)}{\partial b_{j}}=2(y-d)_{ij} 1_{i}
$$
这里隐含这对于 $i$ 求和的意思。由于 $b$ 是一个向量，其与矩阵相加实际上相当于乘了一个长度为 1 的向量。

### 3. 程序来了
这里演示一个昨天里的一个代码：
```python
import numpy as np

# 定义数据
X = np.random.normal(1, 1, [600, 2])
D = X ** 2 + np.random.normal(0, 0.3, [600, 1])

def model(x, w, b):
    """这是我们建立的模型"""
    y = x @ w + b
    return y
def backward(x, d, w, b):
    """以 MSE 为 loss 的求导过程"""
    y = model(x, w, b)
    # dloss/dy
    dloss = 2 * (y-d) / len(x)
    # dw = dloss/dy * dy/dw
    dw = x.T @ dloss
    db = np.sum(dloss, axis=0)
    return dw, db
```
没什么可以解释的了。
