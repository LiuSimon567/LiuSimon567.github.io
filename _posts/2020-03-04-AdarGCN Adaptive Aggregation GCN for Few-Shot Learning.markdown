---
layout:     post
title:      "笔记——AdarGCN Adaptive Aggregation GCN for Few-Shot Learning TRANSFORMATION"
date:       2020-3-4
author:     "SimonLiu"
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# AdarGCN Adaptive Aggregation GCN for Few-Shot Learning
本文提出一种few-shot few- shot learning (FSFSL)问题，解决该问题需要两步，首先使用LDN进行数据扩充与筛选将其转化为传统FSL问题，接着解决该FSL问题。本文中LDN和FSL都借助adaptive aggregation GCN (AdarGCN)的模型进行处理。

![算法原理图](/img/in-post/post-paper-notes-AdarGCN Adaptive Aggregation GCN for Few-Shot Learning/framework.jpg)

1、FSFSL问题
FSFSL问题即真实样本的数目确实很少，imageNet中每个类有600张图片，CUB中每个类少于60张图片，因此需要适当扩充样本。

2、label denoising (LDN)
本文针对FSFSL问题选取的数据扩充方式是爬虫，通过google搜索得到更多图片，但google搜索得到的数据是noisy的，因此使用AdarGCN进行denoising，具体方式是针对每个类别，选取属于该类的正例样本x+、不属于该类的负例样本x-以及属于该类的网络噪声样本x*，噪声样本无法确定是否可当做正例，因此是无标签的，此时可将这三类数据放入一张图中，视为二分类问题，优化网络，最终若噪声样本的预测结果大于阈值，即可视为正例样本。

3、AdarGCN
本文在EGNN的基础上进行改进，首先添加了Can GCNs go as deep as CNNs中的残差模块，其次使用multiHead的node updata策略，其中的2次NU即聚合了two-hop邻居的信息，1次NU即聚合了one-hop邻居的信息，0次NU即还是本身的信息，最后将他们concat在一起，输入EU层进行更新。

![算法原理图](/img/in-post/post-paper-notes-AdarGCN Adaptive Aggregation GCN for Few-Shot Learning/model.jpg)

实验表明残差模块最有用，其次是two-hop邻居的信息。

![算法原理图](/img/in-post/post-paper-notes-AdarGCN Adaptive Aggregation GCN for Few-Shot Learning/exper.jpg)
