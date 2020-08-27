[toc]

# Field-aware Factorization Machines算法详解

## 背景

> 在CTR预估中，经常会遇到one-hot类型的变量，one-hot类型变量会导致严重的数据特征稀疏的情况，此外，特征组合对于CTR的预估起着重要作用。为了解决这一问题，受矩阵分解的启发，FM算法引入特征隐向量，从而有效地解决数据稀疏和特征交叉的问题。但是，在FM算法中，每个特征的隐向量只有一个，而在现实场景中，同一个特征与其他特征交叉的时候，隐向量应该有所不同，例如"Gender=男"这个性别特征与"shop"商家特征和"Ad"广告特征进行交叉的时候存在着差异，基于此，FFM提出了"field-aware"，旨在一个特征学习多个隐向量，用于其他不同特征的交叉。

## 模型

### 公式

> FM算法公式如下，每个特征学习一个k维的隐向量，参数数量为：n*k

img

> FFM算法引入了"field-aware"，每个特征学习不同field的隐向量，参数数量为：n\*f\*k，公式如下：

img

>  <a href="https://www.codecogs.com/eqnedit.php?latex=f_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f_{j}" title="f_{j}" /></a>是第j个特征所属的field。FFM与FM的区别在于隐向量由原来的<a href="https://www.codecogs.com/eqnedit.php?latex=v_{i}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?v_{i}" title="v_{i}" /></a>变成了<a href="https://www.codecogs.com/eqnedit.php?latex=v_{i,&space;f_{j}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?v_{i,&space;f_{j}}" title="v_{i, f_{j}}" /></a>，这意味着每个特征对应的不是唯一的一个隐向量，而是一组隐向量。当<a href="https://www.codecogs.com/eqnedit.php?latex=x_{i}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{i}" title="x_{i}" /></a>特征与<a href="https://www.codecogs.com/eqnedit.php?latex=x_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{j}" title="x_{j}" /></a>特征进行交叉时，<a href="https://www.codecogs.com/eqnedit.php?latex=x_{i}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{i}" title="x_{i}" /></a>特征会从<a href="https://www.codecogs.com/eqnedit.php?latex=x_{i}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{i}" title="x_{i}" /></a>的一组隐向量中选择出与特征<a href="https://www.codecogs.com/eqnedit.php?latex=x_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{j}" title="x_{j}" /></a>的域<a href="https://www.codecogs.com/eqnedit.php?latex=f_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f_{j}" title="f_{j}" /></a>对应的隐向量<a href="https://www.codecogs.com/eqnedit.php?latex=v_{i,&space;f_{j}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?v_{i,&space;f_{j}}" title="v_{i, f_{j}}" /></a>进行交叉。同理，<a href="https://www.codecogs.com/eqnedit.php?latex=x_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{j}" title="x_{j}" /></a>也会选择与<a href="https://www.codecogs.com/eqnedit.php?latex=x_{i}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?x_{i}" title="x_{i}" /></a>的域<a href="https://www.codecogs.com/eqnedit.php?latex=f_{j}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?f_{j}" title="f_{j}" /></a>对应的隐向量<a href="https://www.codecogs.com/eqnedit.php?latex=v_{i,&space;f_{j}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?v_{i,&space;f_{j}}" title="v_{i, f_{j}}" /></a>进行交叉。

### 实例

img

参考：https://www.jianshu.com/p/781cde3d5f3d



### 损失函数

> FFM算法将问题定义为分类问题，损失函数采用的是logistic loss，同时加入了正则项，具体如下：

img

**注意**：此处采用logistic loss的label定义为1和-1，不是常用的1和0，具体可参考：https://www.cnblogs.com/ljygoodgoodstudydaydayup/p/6340129.html

## 实验

> 论文试验了隐向量维度k、正则系数<a href="https://www.codecogs.com/eqnedit.php?latex=\lambda" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\lambda" title="\lambda" /></a>和学习率<a href="https://www.codecogs.com/eqnedit.php?latex=\eta" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\eta" title="\eta" /></a>对FFM模型的影响，具体如下：

img

> 论文在多个数据集上对比了模型的效果，如下：

img

**可以看出，FFM模型的效果达到最优。**

