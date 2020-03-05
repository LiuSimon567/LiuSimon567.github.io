---
layout:     post
title:      "笔记——META-DATASET: A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES"
date:       2020-3-4
author:     "SimonLiu"
header-img: "img/post-build-a-blog.jpg"
tags:
    - 论文笔记
---
# META-DATASET: A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES
本文提出一个新的数据集，集成了十个不同数据集组成的MetaDataset。

1、MetaDataset

本文认为每个episode中的way和shot不应该完全相同，需要具有一定的随机性，从而模拟真实世界中的情况，因此提出一套生成episode的策略。

2、Proto-MAML
本文将MAML和Protonet相结合，提出Proto-MAML的方法。由于本文中episode的way和shot具有随机性，因此MAML方法在测试集的supportset上进行finetune时，最后的FC分类层不能使用MAML训练后得到的初始化值，需要随机初始化，而Protonet其实也可以视为一种使用FC层的方法，如果将MAML最后的FC分类层使用Protonet方法进行初始化，就得到了本文提出的Proto-MAML方法，实验证明该方法效果很好。

![proto-MAML](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/proto-MAML.jpg)

3、评价指标

（1）根据各个数据集上的表现进行排名，最后再计算总排名。其中Proto-MAML第一，protoNet第二，finetune baseline第三。(PS:A BASELINE FOR FEW-SHOT IMAGE CLASSIFICATION中提出的transductive的finetune baseline效果超过了Proto-MAML。)

![proto-MAML](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/rank.jpg)

![proto-MAML](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/rank_transductive.jpg)

（2）基于传统的固定way和shot的episode画出way和shot逐渐增加时acc的变化图，发现way数不变，shot较小时，Proto-MAML和protoNet效果都超过其他方法，但随着shot数的增加，他们迅速饱和，而Finetune baseline、Matching Networks和MAML能从增加的shot中得到更多提高。

![proto-MAML](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/shot_way.jpg)

4、在ImageNet上预训练 vs 在All datasets上预训练

本文认为ImageNet可以作为All datasets的代表，因此比较了在两者上预训练的结果。发现在All datasets上预训练的结果自然好于ImageNet，Omniglot和Quick Draw上的提升最为明显，但在某些数据集上却有下降，可能是由于不同数据集之间差异导致的，该问题还需要进一步研究。

![](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/imagenet_vs_alldataset.jpg)

5、训练时不使用episode vs 训练时使用episode

由于MAML、matchingNet、protoNet在训练时可以不用episodecel，因此本文设计实验来验证episode训练策略的作用，发现在imageNet上预训练时episode策略作用较大，在All datasets上作用不明显，该问题也可能是由于不同数据集之间的差异导致，需要进一步研究。

![](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/episode.jpg)

6、pretrain vs from scratch

本文验证了两种不同预训练方式和from scrach的区别，发现pretrain对大部分数据集是有用的，但也使得Omniglot、Quickdraw、Aircraft上的准确率有所下降，需要注意的是这和4中提升最明显的数据集正好一致，说明pretrain的方法有利于和imageNet相似的数据集。

![](/img/in-post/post-paper-notes-META-DATASET A DATASET OF DATASETS FOR LEARNING TO LEARN FROM FEW EXAMPLES/pretrain_vs_scratch.jpg)



