---
layout: post
title:  "变分自编码器（Variational Autoencoder）（施工中）"
date:   December 5, 2016 10:37 PM
author: Snowkylin
categories: autoencoder
---
记数据点为$X\in\chi$，直接计算$P(X)$：如果$X$像真实图像则概率高，像随机噪声则概率低。
		
我们考虑数据是由一系列**隐变量**(记为$z$)生成的。