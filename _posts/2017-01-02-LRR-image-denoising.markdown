---
layout: post
title:  "使用低秩表示（LRR）进行图像去噪"
date:   January 2, 2017 12:57 PM
author: Snowkylin
categories: LRR image_denoising
---
[上一篇文章](https://snowkylin.github.io/pca/ssc/lrr/2017/01/02/Robust-PCA-SSC-LRR.html)介绍了LRR及相关方法的基本原理。现在，我们使用Extended Yale Face Database B数据集对LRR进行图像去噪能力的测试。参考(Liu, 2010)的方法，从数据集中去掉了光线条件较为极端的图片。同时出于计算效率的考虑，我们选用了100张图片，并将图片大小调整至宽42像素，高48像素。

对于所有的100张图片，我们分别随机添加椒盐噪声。具体而言，遍历每幅图片的每个像素，并以2%的概率将其变为全黑，以2%的概率将其变为全白。

然后，我们将所有100张添加噪声后的图像平展为$ 42 \times 48 = 2016 $维的列向量并进行合并，得到一个$ 2016 \times 100 $大小的矩阵，即LRR中的数据矩阵$ X $。

得到$ X $后，我们调用(Liu, 2010)的作者Liu Guangcan在其个人网站上发布的LRR方法的MATLAB实现，使用exact ALM算法求解以下问题：

$$
\begin{equation}
    \min_{Z, E} \Vert Z \Vert_* + \lambda \Vert E \Vert_1 \; s.t. X = XZ + E
\end{equation}
$$

得到$ Z^* $，并计算$ XZ^* $，将其按列分解为100个2016维向量，再将每个向量还原为$ 42 \times 48 $的图片，从而得到去噪后的图片。

结果如下：

![LRR-Yale-lambda0_1]({{site.url}}/assets/LRR-image-denoising/LRR-Yale-lambda0_1.png)
$ \lambda = 0.1 $时，LRR的图片去噪结果。从左到右分别为：原始图片、加入噪声的图片（$ X $）、LRR恢复后的图片（$ XZ^* $），噪音（$ E $）

![LRR-Yale-lambda0_2]({{site.url}}/assets/LRR-image-denoising/LRR-Yale-lambda0_2.png)
$ \lambda = 0.2 $时，LRR的图片去噪结果。从左到右分别为：原始图片、加入噪声的图片（$ X $）、LRR恢复后的图片（$ XZ^* $），噪音（$ E $）

![LRR-Yale-lambda0_22]({{site.url}}/assets/LRR-image-denoising/LRR-Yale-lambda0_22.png)
$ \lambda = 0.22 $时，LRR的图片去噪结果。从左到右分别为：原始图片、加入噪声的图片（$ X $）、LRR恢复后的图片（$ XZ^* $），噪音（$ E $）

可以注意到，在$ \lambda $的值适合的时候，去噪效果较好，在仅100个数据的条件下也基本完全去除了噪点，且图像质量没有大的损失。以及在本例中，当$ \lambda \geq 0.22 $时，即无法去除噪点。



同时，我们使用BSDS300数据集里的200张照片，使用相同的方式进行了图像去噪测试，结果如下所示。

![LRR-BSDS-lambda0_137]({{site.url}}/assets/LRR-image-denoising/LRR-BSDS-lambda0_137.png)
$ \lambda = 0.137 $时，LRR在BSDS数据集上的图片去噪结果（多次调整参数所能得到的最好结果）。从左到右分别为：原始图片、加入噪声的图片（$ X $）、LRR恢复后的图片（$ XZ^* $），噪音（$ E $）

可见，对于更宽泛的照片范围而言，LRR的表现不够理想。

# 参考

* Guangcan Liu, Zhouchen Lin, Shuicheng Yan, Ju Sun, Yong Yu, and Yi Ma. Robust recovery of subspace structures by low-rank representation. *IEEE Transactions on Pattern Analysis & Machine Intelligence*, 35(1):171–184, 2010.
* K.C. Lee, J. Ho, and D. Kriegman. Acquiring linear subspaces for face recognition under variable lighting. *IEEE Trans. Pattern Anal. Mach. Intelligence*, 27(5):684–698, 2005.
* D. Martin, C. Fowlkes, D. Tal, and J. Malik. A database of human segmented natural images and its application to evaluating segmentation algorithms and measuring ecological statistics. In *Proc. 8th Int’l Conf. Computer Vision*, volume 2, pages 416–423, July 2001.
* Matlab code: solving the low-rank representation (LRR) problems [https://sites.google.com/site/guangcanliu/](https://sites.google.com/site/guangcanliu/)