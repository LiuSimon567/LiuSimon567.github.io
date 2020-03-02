---
layout:     post
title:      "笔记——Graph Few-shot Learning via Knowledge Transfer"
date:       2020-3-2
author:     "SimonLiu"
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# Graph Few-shot Learning via Knowledge Transfer
虽然借用GNN，但本文本质上是一种基于原型网路ProtoNet的方法，因为其GNN只用于生成原型点，接下来的分类仍然借助metric的方法。
![GFL算法原理图](/img/in-post/post-paper-notes-Graph Few-shot Learning via Knowledge Transfer/frame.jpg)

1、利用所有样本，包括support和query构建出一张全局图G，该图无监督训练，损失函数为重构损失。
![重构损失](/img/in-post/post-paper-notes-Graph Few-shot Learning via Knowledge Transfer/Lreg.jpg)

2、借助Hierarchical graph representation learning with differentiable pooling中的diffpool对全局图G进行池化，将细粒度图逐渐粗粒度化，然后每层提取相应信息h，再使用平均值或注意力机制得到从G中抽取出的信息h，最终使用一层FC得到g。但实验结果显示，分层提取h效果并不显著。

![gate示意图](/img/in-post/post-paper-notes-Graph Few-shot Learning via Knowledge Transfer/gate.jpg)

3、对每个类中样本建立一个局部图，边值的集合就是R，R在node节点属性已知的条件下，采用相似度算法得到，如 Jaccard Index、AdamicAdar、PageRank、Top-k CN等。利用2中得到的g与局部图中参数相乘，从而使局部图得到全局图的指导，也就是本文强调的知识迁移。

![生成proto](/img/in-post/post-paper-notes-Graph Few-shot Learning via Knowledge Transfer/proto.jpg)

4、本文启发：能否构建trainset中所有类别的一张全局图，从而对每个episode的局部图参数进行微调，这样相当于每个episode并不独立，利用了全局类与类之间的关系。