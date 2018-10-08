---
layout: post
title:  "Note of Heterogeneous Network Mining"
date:   April 25, 2017 11::08 AM
author: Snowkylin
categories: network-mining heterogeneous-network
---

# 异构网络挖掘 - 目录

本文内容基于 [Prof. 	
Jiawei Han](http://hanj.cs.illinois.edu/) 的 [UIUC CS512: Data Mining: Principles and Algorithms (Spring 2017)](https://wiki.illinois.edu//wiki/display/cs512/Lectures) 课程的Chapter 2。主要基于幻灯片内容做概要性介绍，以后有机会将对其中的若干特定算法另撰文章详细介绍。

<!--more-->

* TOC
{:toc}

# 简介
- 同构网络（homogeneous network）：节点和连边都只有一种
    - 朋友关系网络（顶点为朋友，连边为关系）
    - 万维网（顶点为网页，连边为链接）
- 异构网络（heterogeneous network）：多种节点和连边类型
    - 医疗网络（顶点有医生、患者、病症、治疗方式等）
    - 文献网络（顶点有文献、作者、会议、关键字）
- 同构网络往往由异构网络导出（合作网络由作者-论文的异构网络向作者投影得到）
- 异构网络携带更多信息
- 挖掘半结构化的异构网络
    - 聚类（cluster）
    - 排名（ranking）
    - 分类（classification）
    - 预测（prediction）
    - 相似度搜索（similarity search）

# Part 1：异构网络上的聚类和排名

## 聚类和排名

- RankClus: 在两种节点类型（比如作者和会议）的异构网络上，聚类和排名同时进行并相互提高。
    - Y. Sun, J. Han, P. Zhao, Z. Yin, H. Cheng, T. Wu, "RankClus: Integrating Clustering with Ranking for Heterogeneous Information Network Analysis", EDBT’09
    - 节点连边矩阵：$W = \begin{bmatrix} W_{XX} & W_{XY} \\\\ W_{YX} & W_{YY} \end{bmatrix}$，X是会议，Y是作者。
    - 在聚类中不是每个节点都应该相同对待（重要的节点应该作用越大）
    - 排名在聚类的条件之下（在某聚类内的排名）
    - 排名方法：在不同类型的节点间传播排名数值（类似PageRank）
        1. 高排名的作者在很多高排名的会议发表论文：$r_Y(j) = \sum_{i=1}^{m}W_{YX}(j, i)r_X(i)$
        2. 高排名的会议吸引很多高排名作者的文章：$r_X(i) = \sum_{j=1}^{n}W_{XY}(i, j)r_Y(j)$
        3. 如果一个作者和很多高排名作者合著文章，那么排名将会增加：$r_Y(i) = \alpha \sum_{j=1}^{m}W_{YX}(i, j)r_X(j) + (1 - \alpha)\sum_{j=1}^{n}W_{YY}(i, j)r_Y(j)$ （在1的基础上增加了一项）
    - 其他的排名函数（比如加入具体知识，期刊往往比会议排名高等等）
    - 考虑聚类：从“条件排名分布”到EM框架
        - 对于每个聚类$C_k$，把条件排名分数$r_X \vert C_k$、$r_Y \vert C_k$看做X和Y的条件排名分布（把排名和概率建立关系，并且用这个条件排名分数作为聚类特征来做聚类）
        - 对于对象$o_i$和聚类k，$p(k \vert o_i) \propto p(o_i \vert k)p(k)$。即$o_i$在聚类k里的排名分数$p(o_i \vert k)$越高，则$o_i$在聚类k中的概率$p(k \vert o_i)$越高。
        - 使用EM算法估计参数
            - 根据现有参数$\theta$计算分布$p(z=k \vert y_j, x_i, \theta)$。
            - 根据现在的分布更新$\theta$。
    - EM风格的算法
        - 初始化
        - Repeat
            - 排名：在聚类的子网络内对里面的对象进行排名
            - 对每个对象估计混合模型系数（mixture model coefficients）
            - 调整聚类
        - Until 改变小于阈值
    - 使用Normalized Mutual Info（NMI）评价聚类表现
- NetClus：在星型网络下的基于排名的聚类
    - Y. Sun, Y. Yu, and J. Han, "Ranking-Based Clustering of Heterogeneous Information Networks with Star Network Schema", KDD’09
    - 与RankClus类似的EM风格算法

## 相似度搜索

- 相似度度量/搜索是聚类分析的基础
- Meta-Path（元路径）：元级别的对两个对象之间路径的描述
    - 在Network Schema上的路径，例如“作者-文章-作者”（合作元路径，该作者的学生或者亲密合作者）、“作者-文章-会议-文章-作者”（在类似领域的类似声誉的作者）
    - 不同的Meta-Path有不同的语义信息
- 已有的较流行的网络上相似度度量
    - Personalized PageRank
    - SimRank
- PathSim：异构网络上基于Meta-Path的相似度搜索
    - Y. Sun, J. Han, X. Yan, P. S. Yu, and T. Wu, “PathSim: Meta Path-Based Top-K Similarity Search in Heterogeneous Information Networks”, VLDB'11
    - Peer：在给定meta-path上具有强相连性和相似的可见性（比如在类似领域的类似声誉的作者）
    - 给定一条对称的Meta-Path $\mathcal{P}$，两个对象之间的SimPath定义为：$s(x, y) = \frac{2 \times \vert {p_{x \rightarrow y} : p_{x \rightarrow y} \in \mathcal{P}} \vert}{\vert {p_{x \rightarrow x} : p_{x \rightarrow x} \in \mathcal{P}} \vert + \vert {p_{y \rightarrow y} : p_{y \rightarrow y} \in \mathcal{P}} \vert}$
        - 分子：x和y的相连性
        - 分母：x和y的可见性

## 用户指导的基于Meta-Path的聚类

- 对于不同的聚类目标（不同的Meta-Path偏好），可能期望的结果也会不同
- 问题描述
    - 输入
        - 聚类T的目标类型（即对哪种类型聚类，这里只对单一类型聚类）
        - 聚类数量k
        - 对于一些聚类给定种子：$L_1, \cdots, L_k$（用户指导）
        - 可选的meta-paths：$P_1, \cdots, P_M$
    - 输出
        - 每个meta-path的权值：$w_1, \cdots, w_M$
        - 根据用户指导的聚类结果
- PathSelClus：一个用户指导的基于Meta-Path的聚类概率模型
    - Sun, Yizhou, et al. "Pathselclus: Integrating meta-path selection with user-guided object clustering in heterogeneous information networks." ACM Transactions on Knowledge Discovery from Data (TKDD) 7.3 (2013): 11.


# Part 2：异构网络上的分类和预测

## 分类

- GNetMine
    - 所有对象同等对待
- RankClass：基于排名的分类
    - M. Ji,  M. Danilevsky, and J. Han, "Ranking-Based Classification of Heterogeneous Information Networks", KDD'11
    - 高排名的对象在分类中更重要
    - 排名和分类互相增强
    - 问题描述
        - 给定异构网络$\mathcal{G}$和有分类标签$\mathcal{Y}$的子图$\mathcal{G}'$，预测没有标签的剩下部分$\mathcal{G} - \mathcal{G}'$的分类标签和每类中的排名分布$P(x \vert T(x), k), x \in \mathcal{V}, k = 1 \cdots K$
    - 直觉想法：权威性的传播
        - 连接在一起的对象更可能共享类别k的相似排名分数

## 关系预测

- 同构网络的链接预测——异构网络的关系预测
- PathPredict：基于Meta-Path的关系预测
    - Y. Sun, R. Barber, M. Gupta, C. Aggarwal and J. Han, "Co-Author Relationship Prediction in Heterogeneous Bibliographic Networks", ASONAM'11 
    - 用Meta-Path构成的拓扑特征来预测合作关系（谁会成为你的新的论文合作者）
        - 长度不大于4的Meta-Path（9条，也就是说9个特征）
        - A-P-V-P-A、A-P-A-P-A、A-P→P←P-A最显著
    - 4种度量：路径数（通过某meta-path从作者a到作者b一共有多少路径）、归一化路径数、随机游走、对称随机游走
        - 以上四种混合效果最好
    - 合作时间预测

## 推荐

- 协同过滤受到数据稀疏性的困扰
- 个性化的推荐模型
    - X. Yu, X. Ren, Y. Sun, Q. Gu, B. Sturt, U. Khandelwal, B. Norick, and J. Han, "Personalized Entity Recommendation: A Heterogeneous Information Network Approach", WSDM'14 
    - 基于隐特征的偏好扩散
        - 生成L个不同的Meta-Path连接用户和物品
        - 在各个路径上传播用户的隐性反馈
        - 对每条路径计算用户和物品的隐特征
    - 推荐模型
        - 不同meta-path重要性不同
            - 全局推荐模型：$r(u_i, e_j) = \sum_{q=1}^{L}\theta_q \dot U_i^{(q)}V_j^{(q)T}$
            - r：推荐排名分数
            - q：第q条meta-path，共L条
        - 不同用户可能需要不同模型
            - 个性化推荐模型：$r(u_i, e_j) = \sum_{k=1}^{c}sim(C_k, u_i)\sum_{q=1}^{L}\theta_q \dot U_i^{(q)}V_j^{(q)T}$
            - c：c个软用户聚类
- ClusCite：引用推荐
    - X. Ren, J. Liu, X. Yu, U. Khandelwal, Q. Gu, L. Wang, J. Han,  “ClusCite: Effective Citation Recommendation by Information Network-Based Clustering”, KDD’14
    - 引用推荐：给一篇新的文章（标题、摘要、内容）及其属性（作者、目标会议），推荐一个小集合的高质量引文。

## 网络嵌入

- 任务导向的（task-guided），路径增广的（path-augmented）异构网络嵌入
    - T. Chen and Y. Sun, Task-guided and Path-augmented Heterogeneous Network Embedding for Author Identification, WSDM’17 
    - 问题描述
        - 给定一篇匿名文章（一般是双盲评审），包括会议、年份、关键词、引用列表
        - 预测作者
        - 传统方式：特征工程
        - 新方法：异构网络嵌入

# Part 3：异构网络上的其他数据挖掘方法

## 进化和动力学

- Y. Sun, et al., "Studying Co-Evolution of Multi-Typed Objects in Dynamic Heterogeneous Information Networks", MLG’10

## 角色发现

- DBLP中发掘导师-学生关系
    - Chi Wang, Jiawei Han, Yuntao Jia, Jie Tang, Duo Zhang, Yintao Yu, and Jingyi Guo, “Mining Advisor-Advisee Relationships from Research Publication Networks ", KDD'10

## 图上的OLAP

- Peixiang Zhao, Xiaolei Li, Dong Xin, and Jiawei Han, “Graph Cube: On Warehousing and OLAP Multidimensional Networks”, SIGMOD'11
- Chen Chen, Xifeng Yan, Feida Zhu, Jiawei Han, and Philip S. Yu, "Graph OLAP: Towards Online Analytical Processing on Graphs", ICDM 2008

## 数据清理：同名区分

- 基于链接分析的同名区分
    - X. Yin, J. Han, and P. S. Yu, “Object Distinction: Distinguishing Objects with Identical Names by Link Analysis”, ICDE'07
    - 度量引用之间的相似性
        - 基于链接的相似性：引用同一对象则更可能
        - 基于邻居的相似性：每个引用和其邻居互相相似
    - 自动建立训练集（Self-boosting）
        - 一些很特别的名字一般没有重名

## 多关系数据库间的分类

- CrossMine
    - Xiaoxin Yin, Jiawei Han, Jiong Yang and Philip S. Yu, "Efficient Classification across Multiple Database Relations:A CrossMine Approach", IEEE Trans. Knowledge and Data Engineering, 18(6): 770-783, 2006.

## 多元数据扩展

- EgoSet
    - Xin Rong, Zhe Chen, Qiaozhu Mei, Eytan Adar, “EgoSet: Exploiting Word Ego-Networks and User-Generated Ontology for Multifaceted Set Expansion”, WSDM’16


    





