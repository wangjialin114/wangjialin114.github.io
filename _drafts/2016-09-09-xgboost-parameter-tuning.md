---
layout:     post
title:      "XGBOOST-参数调节<译>"
subtitle:   "常规导弹"
date:       2016-09-09
author:     "WangJialin"
tags:
    - XGBOOST
    - BOOST

---


如果说，DL是核武器的话，我觉得XGBOOST算得上常规导弹了。XGBOOST由陈天奇同学提出，从去年开始在Kaggle比赛上大放异彩。风头虽然比不上DL，可也是上镜率非常高。据说工业界用的也是非常普遍。这段时间打算学习下，顺便拿它来刷刷kaggle试试手。

闲话说完，先礼后兵。让我们慢慢的揭开常规导弹之谜。
避免重复造轮子，还是拿来主义的好。
原文地址：https://xgboost.readthedocs.io/en/latest/how_to/param_tuning.html

##正文

参数调节在机器学习中属于黑科技，一个模型的最优的参数依赖许多场景。因此，想写出一个通用的、有理解力的指南是很难的。

本篇文章试图提供xgboost中的调参的一些有用的指南。

### 理解偏置-方差折中

如果你上过机器学习或者统计课程，这应该是其中一个最重要的概念之一。当我们想模型变得越来越复杂时（例如，深度变大）;模型会有更好的拟合训练数据，生成一个偏置更小的模型。但是，如此复杂的模型需要更多的数据去拟合。

xgboost中的许多参数是关于偏置-方差折中的。最好的模型小心地权衡模型的复杂度和它的预测能力。[**参数文档**](https://xgboost.readthedocs.io/en/latest/parameter.html)中将会告诉你哪些参数将会模型更保守。这可以帮助你更好的在复杂的模型和简单的模型之间取舍。

### 控制过拟合

当你发现训练准确度很高，测试集准确度很低时，这很有可能表明你的模型过拟合了。

通常有两种方法控制xgboost中的过拟合：

- 第一种方法是控制模型的复杂度
	- 包括`max_depth`,`min_child_weight`和`gamma`参数
- 第二种方法是给训练添加随机性，使训练对噪声更鲁棒
	- 包括`subsample`,'colsample_bytree'参数
	- 当然你也可以降低`eta`，但是请记得当你这样做的时候要增大`num_round`


### 处理不平衡的数据集

通常来说，像广告点击这样的数据集，是非常的不平衡的。这种情况会影响xgboost模型的训练，这里有两种方法能够改善这种情况。

- 如果你在意预测的排序（AUC）的话
	- 通过`scale_pos_weight`，平衡正类和负类的权重
	- 使用AUC作为评价指标
- 如果你在意预测概率的正确性的话：
	- 这种情况下，你不能重均衡数据集
	- 这种情况下，设置`max_delta_step`为有限的大小（比如1）来帮助收敛


