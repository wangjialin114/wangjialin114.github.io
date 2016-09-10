---
layout:     post
title:      "tensorflow 学习笔记-Deep MNIST for Expert"
subtitle:   "Expert For Freshman"
date:       2016-08-10
author:     "WangJialin"
tags:
    - tensorflow
    - MNIST
    
---

最近刚开始使用tensorflow，为了练手，打算把官方教程过一遍。
今天照着官方教程[Deep MNIST for Experts][1] 走了一遍，其间也是遇到了一些问题，不过基本上都弄懂了。在这个教程中，搭建的是一个卷积网络CNN。因此核心函数卷积函数的理解是必不可少的。

## 1. tf.nn.conv2d的详解

在此教程中重新定义了以下卷积运算：

``` python
def conv2d(x, W):
    return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding='SAME')
```

API文档：

```
tf.nn.conv2d(input, filter, strides, padding, use_cudnn_on_gpu=None, data_format=None, name=None)

Computes a 2-D convolution given 4-D input and filter tensors.

Given an input tensor of shape [batch, in_height, in_width, in_channels] and a filter / kernel tensor of shape [filter_height, filter_width, in_channels, out_channels], this op performs the following:

    Flattens the filter to a 2-D matrix with shape [filter_height * filter_width * in_channels, output_channels].
    Extracts image patches from the input tensor to form a virtual tensor of shape [batch, out_height, out_width, filter_height * filter_width * in_channels].
    For each patch, right-multiplies the filter matrix and the image patch vector.

In detail, with the default NHWC format,

output[b, i, j, k] =
    sum_{di, dj, q} input[b, strides[1] * i + di, strides[2] * j + dj, q] *
                    filter[di, dj, q, k]

Must have strides[0] = strides[3] = 1. For the most common case of the same horizontal and vertices strides, strides = [1, stride, stride, 1].
Args:

    input: A Tensor. Must be one of the following types: half, float32, float64.
    filter: A Tensor. Must have the same type as input.
    strides: A list of ints. 1-D of length 4. The stride of the sliding window for each dimension of input. Must be in the same order as the dimension specified with format.
    padding: A string from: "SAME", "VALID". The type of padding algorithm to use.
    use_cudnn_on_gpu: An optional bool. Defaults to True.
    data_format: An optional string from: "NHWC", "NCHW". Defaults to "NHWC". Specify the data format of the input and output data. With the default format "NHWC", the data is stored in the order of: [batch, in_height, in_width, in_channels]. Alternatively, the format could be "NCHW", the data storage order of: [batch, in_channels, in_height, in_width].
    name: A name for the operation (optional).

Returns:

A Tensor. Has the same type as input.
```

关于API中参数strides的解读，为什么一定要是[1, x, y, 1]的形式呢？
假设输入的形式是NHWC的格式，即[batch, in_height, in_width, in_channels],strides[0]=1是因为我们通常并不会想跳过batch中的数据，否则的话开始就不应该读入；strides[3]=1对应的是卷积的深度，同样的原因，我们不想跳过输入的数据，因此将其设置为1。

至于padding,这个是为了保证卷积的输出的大小达到我们所需要的目的，设置为'SAME'时，输入和输出的大小就是一样的。


## 2. 为什么最后在建立“Densely Connected Layer”的时候，权重W的大小为[7\*7\*64, 1024]

首先对于7×7×64,64代表前面生成的64个feature， 那为什么是7×7呢？这是因为刚开始图像的大小是28×28, 经过两次pool后，而pool相当于降采样2倍，因此大小就变成了28/2/2等于7。

对于1024,这个值是个经验值，经试验，其实其他的值也是可以的。

## 3. Python Notebook 源码

官方给的都是python文件，并且很有条理，很显然那是经过整合的;对于新手来说，我觉得python notebook真的是太棒了！一行行写，体现了整个思维的形成过程。
我写的代码在[这里](https://github.com/wangjialin114/nlp-tensorflow/blob/master/tensorflow-tutorial/MNIST_for_Experts.ipynb)。

## 参考链接：

- http://stackoverflow.com/questions/34642595/tensorflow-strides-argument
- http://www.cnblogs.com/tornadomeet/p/3258122.html


  [1]: https://www.tensorflow.org/versions/r0.10/tutorials/mnist/pros/index.html#deep-mnist-for-experts
