XGBoost 调参

在这里我主要讲一些对模型性能有影响的参数，如果大家对全部的参数有兴趣，大家可以去[这里](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md)看看。

## General Parameters

- booster（默认gbtree）
	- 可选参数：gbtree，gblinear或者dart。gbtree和dart使用的是基于tree的模型。gblinear使用的是线性函数
- silent（默认0）
	- 0代表打印出运行时信息，1代表静默模式
- nthread（默认最大的线程数）
	- 运行xgboost时的并行线程数

## Parameters for Tree Booster

- eta(默认0.3)
	- 更新中的缩减因子，用于防止过拟合。每学到一棵树，就会将其中的叶子节点的权重乘以此参数，以让后面有更大的学习空间。因此实际中，会将eta设的小一点，学习次数设大一点。越小，树越保守。
	- 范围:[0,1]
- gamma(默认0)
	- 在决定是否进一步分裂一个叶子节点时，损失函数减小的最小值。越大，树越保守。
	- 范围:[0,∞]
- max_depth(默认6)
	- 树的最大深度，增大此值会使树更复杂甚至过拟合
	- 范围:[1,∞]
- min_child_weight(默认1)
	- 孩子节点权重的和的要求的最小值。在树分裂时，如果分裂之后子节点的权重之和小于`min_child_weight`，那么就停止分裂；对于线性回归模型，对应于每个叶子节点至少要有一个实例。值越大，树越保守。

- max_delta_step(默认0)
 