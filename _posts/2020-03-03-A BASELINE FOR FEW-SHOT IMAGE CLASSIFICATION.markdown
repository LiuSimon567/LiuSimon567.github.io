---
layout:     post
title:      "笔记——A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION"
date:       2020-3-3
author:     "SimonLiu"
mathjax:    true
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION
本文提出一个很强的baseline，首先在大规模数据上预训练得到pretrainNet，接着在测试阶段进行finetune，finetune的具体步骤：

1、SUPPORT-BASED INITIALIZATION

在预训练网络的最后的FC分类层后再加上一层分类器，新分类器的输出为此时的类别数，注意是直接加上，而不是去掉最后的FC分类层，在之前的特征层之后加上，这么做的原因是Analyzing and improving representations with the soft nearest neighbor loss中指出网络的最后一层纠缠度较低，具有最好的聚类效果，而特征层的数据纠缠度较高，因此在最后的FC分类层后加分类器，更适用于小样本学习。

而此时新分类器包含参数W和b，w的初始值为各类别之前FC层输出值归一化后的平均值，b的初始值为0，这样可以使得属于该类中的数据激活值最高。
![w_b](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/w_b.jpg)

2、TRANSDUCTIVE FINE-TUNING

虽说本文认为这是一种转导的微调方法，但我认为只是添加了基于最小熵约束的微调而已，各个query的预测仍然是相对独立的，没有利用到相互之间的信息。

![loss](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/loss.jpg)

最小熵的约束在way数很多的情况下效果不好，原因是way数越多，最小熵的数值越大，会超过分类的交叉熵，从而影响结果，因此最小熵需要除以$\log \| C_t \| $进行标准化，$\| C_t \| $为way数。

![loss](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/large_way.jpg)

（META-LEARNING FOR SEMI-SUPERVISED FEW-SHOT CLASSIFICATION中说到转导和半监督其实有联系，如果把转导的那些数据不放到queryset中，那么也就是半监督的方法）

3、本文提出一种评价episode难易程度的指标$\Omega$，并根据它提出新的评价小样本方法好坏的指标，类似AOU，通过计算way数逐渐增多情况下方法准确率曲线下的面积，来衡量方法的好坏。

![omiga](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/hardness.jpg)

![plot](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/plot.jpg)

4、本文还对acc与way、shot之间的关系进行大量实验，结果表明他们之间是对数级关系。
![plot](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/shot_way.jpg)

5、我认为类似baseline的传统训练策略效果好的主要原因是可以在test阶段根据supportset中数据进行finetune，而基于metric的很多方法，例如protoNet、RelationNet不具备这种功能，基于MAML的方法倒是也有这种finetune的过程。（baseline测试时在supportset上finetune通常设定轮数，closelook中设为100轮
MAML测试时也需要finetune，原文中它的finetune在不同数据集不同way shot时都不同，但最多是10轮）

6、本文指出目前miniImageNet数据集split的方式还有区别，很少有使用matchingNet版的split（Matching Networks for One Shot Learning），好像只有他自己使用；其余貌似都使用meta-learning-lstm版的split（OPTIMIZATION AS A MODEL FOR FEW-SHOT LEARNING），例如protoNet、gnn等。
![](/img/in-post/post-paper-notes-A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION/split.jpg)
