---
layout:     post
title:      "tensorflow学习笔记-MNIST Tensorflow Mechanics 101"
subtitle:   "Show me the Code"
date:       2016-08-15
author:     "WangJialin"
tags:
    - tensorflow
    - MNIST
    
---
- [1.关于数据的理解](name="data)
- [2.模型的训练如何体现的](name="train)
- [3.模型控制参数FLAGS相关](name="parameter")
- [4.TensorBoard可视化](name="tensorflow")
- [5.python notebook源码链接](name="code")
- [参考链接](name="ref")

今天把教程[MNIST Tensorflow Mechanics 101][1]过了一遍。

对于想自己把整个代码敲一遍的同学，可能会遇到以下的几个问题：

<a name = "data">  </a>

## 1.关于数据的理解

对于前面的[Deep MNIST for Experts][2]中，label的编码采取的是one-hot编码，因此在后面计算accuracy时，需要采用argmax函数，得到数字（0-9）。而在本教程中label直接就是0-9的数字，不是采用的one-hot编码。
![img](/img/2016_latter_half_year/tensorflow-data.png)
对于这样的数据，后面计算损失函数时，采用的是`sparse_softmax_cross_entropy_with_logits`函数，计算起来非常方便。
![img](/img/2016_latter_half_year/tensorflow-loss.png)

<a name="train"> </a>

## 2.模型的训练如何体现的

在计算图中，我们会定义一个`train_op`,对于这个操作的运行就体现了模型的训练。因此，我们使用sessiion运行时，一定要fetch这个操作。
```python
_, loss_value = sess.run([train_op,loss], 
                         feed_dict={image_placeholder: batch[0], label_placeholder: batch[1]})
```
否则不加上`train_op`的话，模型是得不到训练的，参数也不会更新。

<a name="parameter"> </a>

## 3.模型控制参数FLAGS相关

```
flags = tf.app.flags
FLAGS = flags.FLAGS
flags.DEFINE_boolean("fake_data", False, 'If true, use fake data for unit testing')
flags.DEFINE_string("train_dir", 'data', 'Directory to put the data')
flags.DEFINE_integer("batch_size", 100, "batch size , must be divided evenly into the datasize")
flags.DEFINE_integer('hidden1_units', 128, 'number of units in hidden layer 1')
flags.DEFINE_integer('hidden2_units', 32, 'number of units in hidden layer 2')
flags.DEFINE_integer('max_train_steps', 4000, 'number of the max training steps')
```

这里定义的都是一些模型参数，当然，假如你是自己写代码的话，刚开始也不会知道自己后面要用到哪些参数。可是在ipython notebook时，假如在后面定义的话，你会遇到错误`AttributeError`。
因此，这里最好你还是把参数都在前面一起定义，然后在后面使用。


<a name="tensorboard"> </a>

## 4.TensorBoard可视化

TensorBoard是一个用来帮助我们理解和可视化tensorflow的web应用。目前支持5种可视化，`scalars`, `images`, `audio`, `histograms`, 和`graph`。
对于scalar，我们可以将scalar_summary操作附在我们感兴趣的例如
learning rate 和loss函数的输出节点上。
对于histograms,当我们想可视化某一层的激活函数的分布或者梯度和权重的分布时，可以将histogram_summary操作附在相应的梯度输出和权重变量。
另外，神器，`tf.merge_all_summaries`可以将我们想可视化的所有数据组合到一个操作中，这样使用起来非常方便。
首先是取出session中的summary操作，然后将其传给SummaryWriter用于将其写入事件文件中。这样就可以用tensorboard可视化这些数据。
具体如何使用，可以查看一下[教程](https://www.tensorflow.org/versions/r0.10/how_tos/summaries_and_tensorboard/index.html#serializing-the-data)
对于本教程中的，图如下：
![img](/img/2016_latter_half_year/graph_run.png)
统计的另一个scalar交叉熵如下:
![img](/img/2016_latter_half_year/entropy.png)

<a name="code"></a>

## 5.python notebook源码链接

<a name="ref"></a>

## 参考链接

https://www.tensorflow.org/versions/r0.10/tutorials/mnist/tf/index.html
https://github.com/tensorflow/tensorflow/blob/r0.10/tensorflow/tensorboard/README.md


  [1]: https://www.tensorflow.org/versions/r0.10/tutorials/mnist/tf/index.html
  [2]: https://www.tensorflow.org/versions/r0.10/tutorials/mnist/pros/index.html#deep-mnist-for-experts