---
layout:     post
title:      "笔记——CROSS-DOMAIN FEW-SHOT CLASSIFICATION VIA LEARNED FEATURE-WISE TRANSFORMATION"
date:       2020-3-3
author:     "SimonLiu"
mathjax:    true
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# CROSS-DOMAIN FEW-SHOT CLASSIFICATION VIA LEARNED FEATURE-WISE TRANSFORMATION
本文提出一个即插即用模块——仿射变换层，将其用于MatchingNet、RelationNet、GNN等现有方法，都得到了不错的提升，尤其在跨域数据集上的。

![算法原理图](/img/in-post/post-paper-notes-CROSS-DOMAIN FEW-SHOT CLASSIFICATION VIA LEARNED FEATURE-WISE TRANSFORMATION/framework.jpg)

1、本文提出的利用仿射变换的想法在TADAM已有使用，但具体实现方式不同，本文是在原始网络中添加了仿射变换层，主要优化正态分布的参数$\gamma$和$\beta$，根据该分布再具体生成对应参数。而TADAM中是借用一个复杂的网络来生成$\gamma$和$\beta$，显然TADAM的这种方法更复杂。

2、本文的训练策略与TADAM中不同，仿射变换层的参数f与特征提取层E和度量层M分开更新，主要学习跨域数据上的仿射变换，从而让模型可以适应不同数据集之间特征分布的差异，而TADAM中仿射变换层是根据每个任务生成的，输入该任务的表示（所有类原型的平均），输出$\gamma$、$\beta$的值，和E、M一起更新，不具有适应跨域数据的能力。

3、实验从两方面进行，首先通过人工设定仿射变换层参数证明参数的选取对效果起到重要作用，不好的参数甚至会使结果变糟，接着验证让网络自主学习仿射变换层参数可以得到比人工设定更好的结果。
