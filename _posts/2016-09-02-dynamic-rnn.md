---
layout:     post
title:      "rnn vs dynamic_rnn"
subtitle:   "一静不如一动"
date:       2016-09-02
author:     "WangJialin"
tags:
    - deep-learning
    - tensorflow

---



## 官方API解读

大家在使用tensorflow中的rnn相关函数构建RNN网络时，可能会发现下面两个函数：

```
tf.nn.rnn(cell, inputs, initial_state=None, dtype=None, sequence_length=None, scope=None)

tf.nn.dynamic_rnn(cell, inputs, sequence_length=None, initial_state=None, dtype=None, parallel_iterations=None, swap_memory=False, time_major=False, scope=None)
```
并且可能会非常犹豫到底选用哪个函数。

首先，要明确一点，这两个函数的功能是一样的。

```
可以肯定，后面tf.nn.rnn这个函数一定会被淘汰，在任何时候你直接用tf.nn.dynamic函数就OK,效率真的高很多。
```
只不过dynamic_rnn函数可以将输入完全的动态展开(fully dynamic unrolling of inputs)。

对于rnn函数，rnn是静态的展开的，可是支持dynamic computation。
`dynamic computation`是指图是展开到`max_sequence_length`的，可是如果在参数中提供`sequence_length`的话，那么使用条件控制计算只会计算到`sequence_length`，这样也许可以节省时间。

`dynamic unrolling of inputs`则是不同的含义，它是指图根据`sequence_length`的长度来进行展开。在后面我做的一个简单的实验比较中就可以发现，**这种动态展开真的很节约时间**。

对于rnn，为了在`max_sequence_length`处获得正确的状态，通常是给输入的左边补零右对齐，这样的话，它的计算就是从`len(inputs)-max(sequence_length)`处开始的。


说到这，你可能似懂非懂。OK， I will show you the code.

## 实现的区别

代码在[这里](https://github.com/wangjialin114/nlp-tensorflow/blob/master/sentiment_analysis/polarity_analysis/models/)

具体的一些项目信息你可以在github上面看到。

当你在看上面两个函数的用法后，一定要注意，以下几点，千万不要以为直接将`tf.nn.rnn`换成`tf.nn.dynamic_rnn`就好了。有以下几点是需要同时作出变化的：
看下面之前，第一定要好好看下官方的API说明。

```
tf.nn.rnn:
- 输入inputs:长度为T的list， 每个list的元素都是[batch_size, in_channels]的tensor
- outputs:长度为T的list,因此最终的输出为outputs[-1]
- state:长度为T的list，因此，最终状态为state[-1]
```

```
tf.nn.dynamic_rnn:
在time_major=False的情况下，
- 输入inputs:一个tensor，大小为[batch_size, time_stamp in_channels
- outputs:一个Tensor, 大小为[batch_size, time_stamp in_channels]
- state:final state
```

因此，你构造输入的方式要变化，你取最终的输出和输出状态时也要进行相应的改变。

```
如果你不做出改变的话，会出现`Incompatitable for shape`的错误。
```

在构建了一个T=400, batch_size = 20，hidden_size=100的网络后，运行比较发现，两者时间真的相差很多，dynamic简直太棒了，下面是我程序中同样的设置，比较运行的一个结果。

```
lstm fixed inputs unrolling : 458.973615 seconds, lstm dynamic inputs unrolling: 124.159002 seconds
```