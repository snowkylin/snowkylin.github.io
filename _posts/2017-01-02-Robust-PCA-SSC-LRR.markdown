---
layout: post
title:  "Robust PCA, SSC and LRR"
date:   January 2, 2017 1:31 AM
author: Snowkylin
categories: PCA SSC LRR
---

# Robust PCA，稀疏子空间聚类（SSC）与低秩表示（LRR）

低秩表示（Low-Rank Representation, LRR）方法由Liu等于2010年提出，是用于解决子空间分割等问题的一种有效方法，通过在Robust PCA的基础上引入子空间聚类的思想，加入字典项使Robust PCA中的单一低秩子空间变为多个子空间的集合，从而达到更好的效果。

<!--more-->

* TOC
{:toc}

# PCA与Robust PCA
主成分分析（PCA）是我们熟知的降维方法，能有有效找出数据中的主要结构，去除噪音，在一组新的基下尽量表达原有数据间的关系。不过，PCA方法对于大的噪声或者严重的离群点（Outlier）比较敏感而难以正常工作，而Robust PCA正解决了这一问题。Robust PCA假设噪声$ E $是稀疏的（而不太关心噪声的强度），优化以下表达式：

$$
\begin{equation*}
    \min_{Z, E} rank(Z) + \lambda \Vert E \Vert_0 \; s.t. X = Z + E
\end{equation*}
$$

其中，$ rank(\cdot) $表示矩阵的秩，$ \Vert\cdot\Vert_0 $表示矩阵的L0范数（矩阵非0元素的个数）。A是包含真实结构的低秩矩阵，E是稀疏噪声。考虑线性代数中，矩阵的秩度量的是矩阵行列之间的相关性。如果矩阵是低秩的，说明这个矩阵各行/列的相关性很强，从而可以投影到更低维的线性子空间，用更少的若干向量就可以表达。而人脸图像、用户-物品表格等矩阵正具有这种强的相关性，因此在理想状态下一般是低秩的。不过，如果低秩矩阵$ Z $受到随机稀疏噪声$ E $的影响，那么$ Z $的低秩性就被破坏了，变为了我们实际观察到的高秩矩阵$ X $。而Robust PCA想要做的，正是从这个$ X $中寻找尽可能低秩的$ Z $和尽可能稀疏的$ E $。

然而，矩阵的秩$ rank(\cdot) $和L0范数$ \Vert\cdot\Vert_0 $非凸且非光滑，难以进行优化，因此我们一般使用矩阵的核范数$ \Vert\cdot\Vert_* $（矩阵奇异值的和）和L1范数$ \Vert\cdot\Vert_1 $对其进行松弛，从而得到一个凸优化问题：

$$
\begin{equation*}
	\min_{Z, E} \Vert Z \Vert_* + \lambda \Vert E \Vert_1 \; s.t. X = Z + E
\end{equation*}
$$

# 稀疏子空间聚类（Sparse Subspace Clustering, SSC）

稀疏子空间聚类（Sparse Subspace Clustering, SSC）由Elhamifar等于2009年提出，其优化如下表达式：

$$
\begin{align*}
    min \Vert &z_i \Vert_0 \\
    s.t. \; &x_i = X_{\hat{i}}z_i, \forall i \\
    where \; &X_{\hat{i}} = [x_1, ..., x_{i - 1}, x_{i + 1}, ..., x_n]
\end{align*}
$$

也就是说，迫使每个数据$ x_i $只用其他数据（不包括自身）的线性组合来表示，且使用其他数据越少（即表达越稀疏）越好。上式用矩阵形式表示为：

$$
\begin{align*}
    min \Vert &Z \Vert_0 \\
    s.t. \; &X = XZ, Z_{ii} = 0
\end{align*}
$$

使用L1范数对上式中的L0进行松弛，并加入正则项，得：

$$
\begin{align*}
    min \Vert &Z \Vert_1 + \lambda \Vert E \Vert_1\\
    s.t. \; &X = XZ + E, Z_{ii} = 0
\end{align*}
$$

上式的解$ Z $具有块对角结构，这种结构解释了数据的子空间属性：块的个数代表子空间个数，每个块的大小代表对应子空间的维数，同一块的数据属于同一子空间。

# 低秩表示（Low-Rank Representation, LRR）

考虑前面的Robust PCA，我们可以注意到，Robust PCA隐性地假定数据中的主要结构是在一个单一的低秩子空间里的。然而如果数据是从多个子空间<span markdown='0'>$ \mathcal{S}_1, \mathcal{S}_2, ..., \mathcal{S}_k $中取样得到的呢？Robust PCA会将它们当做是从$ \mathcal{S} = \sum_{i=1}^{k}\mathcal{S}_i $中取样得到的。然而，$ \mathcal{S} $很可能比实际上的$ \bigcup_{i=1}^{k}\mathcal{S}_i $要大得多，使得结果不够准确。LLR在Robust PCA的基础上通过引入一个字典矩阵$ A $，将这种多子空间的情况纳入考量，表述如下：</span>

$$
\begin{equation*}
	\min_{Z, E} \Vert Z \Vert_* + \lambda \Vert E \Vert_1 \; s.t. X = AZ + E
\end{equation*}
$$

在一些特定任务中，我们可以选择$ A = X $，则上式可以表述为：

$$
\begin{equation*}
    \min_{Z, E} \Vert Z \Vert_* + \lambda \Vert E \Vert_1 \; s.t. X = XZ + E
\end{equation*}
$$

可见其形式与SSC类似（然而没有$ Z_{ii} = 0 $的约束）。事实上，其也确实能够很好地解决稀疏子空间聚类的问题。简单地说，我们大概可以将LRR理解为Robust PCA和SSC两种方法的融合。

# 参考

* Guangcan Liu, Zhouchen Lin, Shuicheng Yan, Ju Sun, Yong Yu, and Yi Ma. Robust recovery of subspace structures by low-rank representation. *IEEE Transactions on Pattern Analysis & Machine Intelligence*, 35(1):171–184, 2010.
* E. Elhamifar and R. Vidal. Sparse subspace clustering. In *IEEE Conference on Computer Vision and Pattern Recognition, 2009. CVPR 2009.*, pages 2790–2797, 2009.
* 机器学习中的范数规则化之（二）核范数与规则项参数选择[http://blog.csdn.net/zouxy09/article/details/24972869](http://blog.csdn.net/zouxy09/article/details/24972869)
* Matlab code: solving the low-rank representation (LRR) problems [https://sites.google.com/site/guangcanliu/](https://sites.google.com/site/guangcanliu/)