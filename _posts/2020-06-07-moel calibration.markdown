---
layout:     post
title:      "模型概率校准——model calibration"
date:       2020-6-7
author:     "SimonLiu"
mathjax:    true
header-img: "img/post-build-a-blog.jpg"
tags:
    - 专题思考
---

# 模型校准的含义

模型校准的含义是将模型最终的输出转化为概率，也称为置信度。[1]中介绍了通常SVM、BST等算法需要校准，而神经网络、bagged tree、random forest、逻辑回归等算法会好一些，常用的评价指标是reliability diagram，如下图所示，reliability diagram越靠近对角线则说明模型校准效果越好，反之则校准效果较差。常用的校准算法有Platt Calibration 和 Isotonic Regression等。

![](/img/in-post/post-paper-notes-model calibration/reliability diagram.jpg)

# 模型校准与准确率

其实神经网络输出的概率也是需要校准的，我们通常将网络最后一层经过softmax后的输出作为每个类别的置信度[2]，但其实这是不妥的，因为网络的错误预测置信度也可能会很高，这部分是准确率acc所无法衡量的。而在某些任务场景下该置信度很重要，我们更希望模型对于其不确定的数据尽可能谨慎地判断，从而可以设置阈值进行人工的二次判断。换句话说，我们希望模型的预测尽量是对的，哪怕有很多数据没法做出判断也没关系。

# 模型校准的方法

一些常用的模型校准的方法[3]：Monte Carlo Dropout、Deep Ensemble、Temp Scaling

[2]中提到一种校准方法，先训练几十个常用模型，然后从中挑三个表现较好的模型，同时这些模型的结构最好差异较大，接着将选出的模型继续精调，最后使用投票的方式，只有三个模型预测一致，置信度才为1，并且将阈值设置为1。这样模型作出的判断基本可以认为是对的，不对的都人为进行二次判断。

>参考文献
>
>[1] [模型校准calibration](https://zhuanlan.zhihu.com/p/101766505) 
>
>[2] [CNN入门讲解：准确率很高就感觉自己萌萌哒？NONONO,还有一点也重要](https://zhuanlan.zhihu.com/p/38245449)
>
>[3] [uncertainty中常见的method和metric](https://zhuanlan.zhihu.com/p/110687124)