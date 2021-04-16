---
layout: post
title:  Multi-path Learning for Object Pose Estimation Across Domains
date:   2019-08-11
author: wanger
header-img: 
tags: 
  - 
---

# Multi-path Learning for Object Pose Estimation Across Domains

### 论文框架
![models](../img/811/model.png)

论文的模型思路来自于[Augmented Autoencoder (AAE) ](http://openaccess.thecvf.com/content_ECCV_2018/papers/Martin_Sundermeyer_Implicit_3D_Orientation_ECCV_2018_paper.pdf)。通过训练一个Autoencoder，来学习物体6D姿态的潜在信息。进而利用潜在编码进行姿态求解。
不同于AAE的是，这篇论文使用的是多路解码器，encoder部分输入是数据增强以后不同角度的RGB图像，解码部分则是每个物体设置一个解码器。

### AAE介绍
定义输入图片 $\mathbf{x}\in \mathcal{R}^d$， 数据增强操作为 $f(\cdot)$, encoder为 $\Upsilon(\cdot)$, 潜变量为 $\mathbf{z}\in \mathcal{R}^m, (m \ll d)$， decoder 为 $\Lambda(\cdot)$. 重建图像 $\hat{\mathbf{x}}$

$$
\hat{\mathbf{x}}=\Lambda(\Upsilon(f(\mathbf{x})))=\Lambda\left(\Upsilon\left(x^{\prime}\right)\right)=\Lambda(\mathbf{z})
$$

loss 通常为：
$$
\ell_{2}(\mathcal{B})=\sum_{i \in \mathcal{B}}\left\|\mathbf{x}_{i}-\hat{\mathbf{x}}_{i}\right\|_{2}=\sum_{i \in \mathcal{B}}\left\|\mathbf{x}_{i}-\Lambda\left(\Upsilon\left(f\left(\mathbf{x}_{i}\right)\right)\right)\right\|_{2}
$$

### Multi-Path Encoder-Decoder
多路decoder的loss 定义为
$$
\begin{aligned} \ell_{m}(\tilde{\mathcal{B}}) &=\sum_{j=1}^{b} \sum_{k=1}^{n} \mathbb{I}\left(s_{j}=k\right)\left\|\mathbf{x}_{j}-\Lambda_{k}\left(\Upsilon\left(f\left(\mathbf{x}_{j}\right)\right)\right)\right\|_{2} \\ &=\sum_{j=1}^{b} \sum_{k=1}^{n} \mathbb{I}\left(s_{j}=k\right)\left\|\mathbf{x}_{j}-\Lambda_{k}\left(\mathbf{z}_{j}\right)\right\|_{2} \end{aligned}
$$
其中$\mathbb{I}$是指示函数。

### Object Pose Estimation Across Domains

### Iterative Reﬁnement of Latent Codes
![models](../img/811/pose.png)