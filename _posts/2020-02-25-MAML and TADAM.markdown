---
layout:     post
title:      "笔记——MAML和TADAM"
date:       2020-2-25
author:     "SimonLiu"
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# META-LEARNING WITH LATENT EMBEDDING OPTIMIZATION
本文主要在MAML的基础上进行改进。

1、MAML直接在高维空间进行第一步梯度更新，而LEO是映射到低维空间进行第一步梯度更新，然后再映射回高维空间。


![LEO原理算法图](/img/in-post/post-paper-notes-MAML and TADAM/LEO.jpg)

2、MAML的参数没有随机性，而LEO借助生成器，生成符合正态分布的参数，进一步增强泛化能力。
![](/img/in-post/post-paper-notes-MAML and TADAM/generator1.jpg)
![](/img/in-post/post-paper-notes-MAML and TADAM/generator2.jpg)

# TADAM: Task dependent adaptive metric for improved few-shot learning
本文主要在原型网络的基础上进行改进、

![TADAM原理算法图](/img/in-post/post-paper-notes-MAML and TADAM/TADAM.jpg)


1、Metric Scaling：在similarity metric的基础上学习一个scaling factor，这样能够更好的使得输出的metric在合适的范围内，当\alpha趋近于0时候，metric函数会拉近query与其对应类中心的距离，拉远所有与其不对应类中心的距离，而当\alpha趋近于无穷的时候，metric函数仍然拉近query与其对应类中心的距离，但会拉远query与其最难区别的不对应类中心的距离，类似soft triple loss和hard triple loss之间的关系。

![metric Scaling](/img/in-post/post-paper-notes-MAML and TADAM/metric.jpg)

2、Task Conditioning：构造一个TEN network，通过输入样本数据来得到task representation，并利用此作为condition得到$\alpha$、$\beta$来改变feature extractor的输出，也就是使得每一个task的feature extractor都不一样，具备adaptation的能力,此处的task representation使用各个类原型的平均来表示。

![TEN框架图](/img/in-post/post-paper-notes-MAML and TADAM/TEN.jpg)

3、Auxiliary task co-training: 用传统训练方法生成batch数据，和episode策略一起训练网络，不仅能加快网络收敛速度，还能得到更好的结果。

AM3将其中TEN的输入由该任务中所有类原型的均值变为语义信息。