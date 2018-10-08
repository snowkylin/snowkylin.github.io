---
layout: post
title:  "HopField Network and Restricted Boltzmann Machine (RBM)"
date:   January 2, 2017 3:10 PM
author: Snowkylin
categories: RBM HopField
---

# HopField网络与受限玻尔兹曼机（RBM）

本文简要介绍HopField网络和受限玻尔兹曼机（Restricted Boltzmann Machine, RBM）的原理。

<!--more-->

* TOC
{:toc}

# HopField网络

HopField网络是一种循环式的神经网络，从输出到输入有反馈连接。信号输入后，神经元的状态会随时间而不断变化，最后收敛或周期性震荡。

<center>
<img src="{{site.url}}/assets/HopField-RBM/Hopfield-net.png" width="50%"/><br/>
一个有4个节点的HopField网络
</center>

设共有N个神经元，$ x_i $为第i个神经元的输入，$ w_{ij} $为神经元i和j之间的权值（在无自反馈型HopField网络中，$ w_{ii} = 0$，即神经元不与自己连接；$ w_{ij} = w_{ji} $，即权重矩阵对称），$ \theta_i $为第i个神经元的阈值，第i个神经元在时间t的状态为$ y(i, t) $，则有以下递推式：

$$
\begin{align}
    u(i, t+1) &= \sum_{j=1}^{N}w_{ij}y(j, t) - \theta_i \\
    y(i, t+1) &= f(u(i, t+1))	\label{eq1.2}
\end{align}
$$

为简单起见，我们考虑离散型HopField网络，即$f(x)$为sgn函数。在此，我们引入能量的概念，定义能量的“增量”为：

$$
\begin{align}
    \Delta E(i, t_1, t_2) 	&= -u(i, t)(y(i, t_1) - y(i, t_2)) \\
                            &= -u(i, t)\Delta y(i, t_1, t_2) 
\end{align}
$$

此处，y只有1和-1两种取值。当$ y(i, t_1) = 1 $时，$ u(i, t_1) > 0 $，$ y(i, t_1) - y(i, t_2) > 0 $，故$ \Delta E(i, t_1, t_2) < 0 $。同理，当$ y(i, t_1) = -1 $时，$ u(i, t_1) < 0 $，$ y(i, t_1) - y(i, t_2) < 0 $，故$ \Delta E(i, t_1, t_2) < 0 $。因此，只要神经元i的状态y发生变化（无论是从1到-1还是从-1到1），能量变化值$ \Delta E $都会为负，即能量变小。由此我们可以看出，神经网络的变化过程实质上是一个能量不断减小的过程。当HopField网络达到稳定时，能量函数最小。

我们定义t时刻神经元i的能量为：

$$
\begin{align}
    E(i, t) &= -\frac{1}{2}u(i, t)y(i, t) \nonumber\\
            &= -(\sum_{j=1}^{N}w_{ij}y(j, t) - \theta_i)y(i, t)
\end{align}
$$

则总能量为：

$$
\begin{align}
    E(t) &= \sum_{i=1}^{N}E(i, t) \nonumber\\
         &= \sum_{i=1}^{N}-\frac{1}{2}(\sum_{j=1}^{N}w_{ij}y(j, t) - \theta_i)y(i, t) \nonumber\\
         &= -\frac{1}{2}\sum_{i=1}^{N}\sum_{j=1}^{N}w_{ij}y(j, t)y(i, t) + \sum_{i=1}^{N}\theta_i y(i, t)
\end{align}
$$

通过HopField网络，我们了解了“神经网络的能量函数”这一概念。在此基础上，我们讨论受限玻尔兹曼机。

# 受限玻尔兹曼机

<center>
<img src="{{site.url}}/assets/HopField-RBM/Restricted_Boltzmann_machine.png" width="50%"/><br/>
包含三个可见单元和四个隐单元的受限玻尔兹曼机示意图（不包含偏置节点）
</center>

在受限玻尔兹曼机（RBM）中，我们将神经元分为两部分，分别为可见单元集合v和隐藏单元集合h。可见单元集合用于接受输入，隐藏单元集合用于提取特征。可见单元集合和隐藏单元集合之间全连接，而集合内部的神经元均无连接，构成一个完全二分图，如上图所示。我们重新推导RBM的总能量函数如下：

$$
\begin{align}
    E(t) &= \sum_{i \in v}E(i, t) + \sum_{i \in h}E(i, t) \nonumber\\
         &= \sum_{i \in v}-\frac{1}{2}(\sum_{j=1}^{N}w_{ij}y(j, t) - \theta_i)y(i, t) + \sum_{i \in h}-\frac{1}{2}(\sum_{j=1}^{N}w_{ij}y(j, t) - \theta_i)y(i, t) \nonumber\\
         &= \sum_{i \in v}-\frac{1}{2}(\sum_{j \in h}w_{ij}y(j, t) - \theta_i)y(i, t) + \sum_{i \in h}-\frac{1}{2}(\sum_{j \in v}w_{ij}y(j, t) - \theta_i)y(i, t) \nonumber\\
         &= -\sum_{i \in v}\sum_{j \in h}w_{ij}y(j, t)y(i, t) + \sum_{i=1}^{N}\theta_i y(i, t)
\end{align}	
$$

定义神经元i的偏置$ b $为神经元阈值$ \theta $的相反数，即$ b_i = -\theta_i $，代入上式。同时，为后文叙述方便起见，省略参数t，得：

\begin{equation}
    E = -\sum_{i \in v}\sum_{j \in h}w_{ij}y(j)y(i) - \sum_{i=1}^{N}b_i y(i) \label{eq1.7}
\end{equation}

在这里，我们借用统计力学中的“正则分布”[^1]，定义神经元状态$ (y_1, y_2, ..., y_N) $的联合概率分布为：

\begin{equation}\label{eq1.8}
    P(y_1, y_2, ..., y_N) = \frac{1}{Z}e^{-E(y_1, y_2, ..., y_N)}
\end{equation}

由于$ v + h = \{y_1, y_2, ..., y_N\} $，上式也可记为$ P(v, h) = \frac{1}{Z}e^{-E(v, h)} $。其中$ Z = \sum_{v, h}e^{-E(v, h)} $是归一化因子。此处累加号下的$ v+h $代表遍历$ v+h $这N个神经元的所有状态。（如果每个神经元都是二元取值，则为$ 2^N $个）

由于RBM具有二分图结构，其具有一个重要性质，即当可见单元集合v给定时，隐藏单元集合的每个神经元的分布独立，反之亦然。[^2]使用公式表示如下：

$$
\begin{align}
    P(h|v) = \prod_{y_i \in h}P(y_i|v) \label{eq1.9}\\
    P(v|h) = \prod_{y_i \in v}P(y_i|h) \label{eq1.10}
\end{align}
$$

给定一个训练样本v（输入到可见单元集合），我们关心这个样本v在RBM中的概率分布$ P(v) $，即$ P(v, h) $的边缘分布（也称为似然函数）。具体而言，$ P(v) $的计算可以通过遍历隐藏单元集合$ h $的所有状态（$ 2^{\\#(h)} $种），将每个取值生成v的概率累加。表达式为：

\begin{equation}
    P(v) = \sum_{h}P(v, h) = \frac{1}{Z}\sum_{h}e^{-E(v, h)}
\end{equation}

此处累加号下的$ h $代表遍历隐藏单元$ h $的所有状态（$ 2^{\\#(h)} $种）。

给定一组训练样本$ V = \{v_1, v_2, ..., v_M\} $（训练样本输入到可见单元集合v，所以两者使用了相同的字母），我们的目标是调整RBM的参数，使得RBM的概率分布尽可能与训练数据相符，即最大化如下似然函数：

\begin{equation}
    \mathcal{L}(V) = \prod_{v \in V}P(v)
\end{equation}

其对数形式为：

$$
\begin{align}
    \ln\mathcal{L}(V) 	&= \sum_{v \in V}\ln P(v) \nonumber \\
                        &= \sum_{v \in V}\ln (\frac{1}{Z}\sum_{h}e^{-E(v, h)}) \nonumber \\
                        &= \sum_{v \in V}(\ln \sum_{h}e^{-E(v, h)} - \ln Z) \label{eq1.13}
\end{align}
$$

可以使用梯度上升法最大化上式[^3]，但效率过低。Hinton于2002年提出了对比散度（Contrastive Divergence, CD）算法，大幅改进了计算效率，成为目前训练RBM的标准算法。

# 参考

* G. E. Hinton, S Osindero, and Y. W. Teh. A fast learning algorithm for deep belief nets. *Neural Computation*, 18(7):1527–1554, 2006.
* 限制玻尔兹曼机（Restricted Boltzmann Machine）学习笔记<http://blog.csdn.net/roger__wong/article/details/43374343>
* 受限玻尔兹曼机（RBM）学习笔记<http://blog.csdn.net/itplus/article/details/19168937>

# 注释

[^1]:统计力学的一个基本结论是，系统与外界达到热平衡时，系统处于状态i的概率$ p_i $具有以下形式：$p_i=\frac{1}{Z_T}exp(-\frac{E_i}{T})$。其中$ Z_t = \sum_{i}exp(-\frac{E_i}{T}) $是归一化常数。T为正数，表示系统所处的温度。从这个分布可以看出，同一温度下能量越小的状态具有越大的概率。而当温度T升高时，概率分布对能量越来越不敏感，逐渐趋于均匀分布。
[^2]:详细推导可见<http://blog.csdn.net/itplus/article/details/19168989>
[^3]:详细推导可参考<http://blog.csdn.net/itplus/article/details/19207371>
