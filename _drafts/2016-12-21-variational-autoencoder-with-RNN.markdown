---
layout: post
title:  "基于RNN的变分自编码器（施工中）"
date:   December 21, 2016 8:30 PM
author: Snowkylin
categories: autoencoder RNN
---
（在这篇文章之前，你需要了解一些关于RNN的内容，推荐[这篇](http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/)）

[之前](https://snowkylin.github.io/autoencoder/2016/12/05/introduction-to-variational-autoencoder.html)介绍了变分自编码器（Variational Autoencoder, VAE）的基本原理。VAE是一个可以广泛使用的方法，这篇文章将介绍如何将VAE与循环神经网络（RNN）结合起来，实现一个“渐进生成”的变分自编码器。这应该也可以算是RNN的一个很特别的应用。Google Deepmind在2015年发表的[DRAW: A Recurrent Neural Network For Image Generation](http://arxiv.org/abs/1502.04623)就是基于这种原理实现了效果良好的图像生成（不过DRAW还加上了Attention机制）。

我们知道，变分自编码器的结构是“原始输入-（编码）-压缩表示-（解码）-重构输入”。一般而言，重构输入是一步生成的。不过在现实生活中，我们如果要重构输入（比如画画），是不可能像盖章一样一步重构输入的，而是“一笔一笔地”分次构建，逐步完善，最后成为完整的作品，这也就是VAE+RNN的intuition。我们希望通过RNN让重构输入“渐进生成”，一次生成一部分，最后成为完整的重构输入。

用图表示大概如下：

![VAE-RNN]({{site.url}}/assets/vae-rnn/vae-rnn.png)

（左边是一般的VAE，右边是加入RNN的VAE，例图摘自[DRAW: A Recurrent Neural Network For Image Generation](http://arxiv.org/abs/1502.04623)）


