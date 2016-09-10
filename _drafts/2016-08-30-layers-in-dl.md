## 深度学习中的一些layers

RNN Layer

CNN Layer

Fully-Connected Layers

Pooling Layers

Dropout Layer
Dropout Layer会基于给定的概率随机将元素设置为0。这对应于临时的将选定的单元和它的所有连接在训练网络中丢掉。因此，对于每个新的输入，会选择神经元的不同子集，这样就形成了一个不同的网络。同时，这些不同的网络事实上使用的是共同的权重，但是由于不依赖某个特定的神经元和连接，因此增加dropout layer可以防止过拟合。
[1] Srivastave, N., G. Hinton, A. Krizhevsky, I. Sutskever, R. Salakhutdinov. "Dropout: A Simple Way to Prevent Neural Networks from Overfitting." Journal of Machine Learning Research. Vol. 15, pp. 1929-1958, 2014.

Embedding Layer
Embedding Layer将代表索引的整数转换成一个固定大小的向量。通常是在NLP中用的比较多。用数学公式表示就是：
$$

Normalize Layer



