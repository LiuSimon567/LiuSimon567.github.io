---
layout:     post
title:      "笔记——PPN和Large-Scale Few-Shot Learning Knowledge Transfer With Class Hierarchy"
date:       2020-3-6
author:     "SimonLiu"
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# Prototype Propagation Networks (PPN) for Weakly-supervised Few-shot Learning on Category Graph

本文算是protoNet的改进，借助分级的Category Graph，将信息在各级原型点之间传播。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/framework1.jpg)

1、建立分级的Category Graph
由于imageNet中的class是分层的，在wordNet的基础上建立，如果把miniImageNet中的类别看做叶子结点，那么很容易向上得到父节点和祖先节点，从而构造出分级的Category Graph。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/category.jpg)

2、计算各级原型点，并传播信息。
初始化原型点P0，子类、父类和祖先类都是求类中所有元素的平均值。
![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/p0.jpg)

接着利用注意力机制，计算所有父节传播给子节点的信息P+。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/p+.jpg)

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/att.jpg)

最后将P0与P+融合，得到最终子类更新后的原型表示P。（父类与祖先类不更新）

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/p.jpg)

注意该方法最终需要优化的参数只有cnn和att两个模块。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/algorithm.jpg)

3、PPN和PPN+
train时父类与祖先类原型的信息在test时是可获取的.

PPN+:test时知道具体的父类祖先类，若父类祖先类没有出现在train中，则对该类中所有样本求平均得到P。

PPN:test时不知道具体的父类祖先类，根据KNN选出最近的K个父类祖先类，从而进行信息传播。

4、实验结果

本文中的Category Graph也可用于protoNet和GNN等，但效果提升不大(不知道具体怎么应用的，只是简单的加上父类祖先类标签进行分类吗？)

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/exp.jpg)

5、future work

（1）本文中只借助att用了一步信息传播，可不可以实现多步。

（2）本文构建的是Category Graph，可不可以构建Attribute Graph甚至其他可迁移的有用信息的graph。


# Large-Scale Few-Shot Learning Knowledge Transfer With Class Hierarchy

本文主要是在特征提取上作文章，最后的分类借助protoNet中的metric方法，其实还可以用其他metric的方法。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/framework2.jpg)

1、Class Hierarchy

minImageNet中的每个类名称在glove2vec中都可以找到，因此借助glove2vec对类名称进行K-means的聚类。文中实验过不同的分层聚类数，最红发现分三层，分别为底层叶子结点200类，中间层40类和顶层8类，效果最好。

本文构建Class Hierarchy使用了target classes，但其实不使用target，也能达到不错的效果。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/k-means.jpg)

2、特征提取模块

train时使用传统的训练策略，而最终test时，只用其中的CNN部分。

![](/img/in-post/post-paper-notes-PPN and Large-Scale Few-Shot Learning Knowledge Transfer With Class/feature.jpg)

3、本文test时因为使用metric的方法，所以没有对CNN进行微调，其实可以用传统训练法，借助supportset进行微调，不知效果如何。但也有可能正是因为固定住CNN，才使得train时的knowledge得以很好地transfer。

#总结

这两篇文章想法有共通之处，都借助了父类祖先类（超类）使网络学习到的知识更好地迁移到没有见过的新类上，那么这种超类的思想能否与GNN相结合呢？