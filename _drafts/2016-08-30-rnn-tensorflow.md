

## rnn vs dynamic_rnn
大家在使用tensorflow中的rnn相关函数构建RNN网络时，可能会发现下面两个函数：

```
tf.nn.rnn(cell, inputs, initial_state=None, dtype=None, sequence_length=None, scope=None)

tf.nn.dynamic_rnn(cell, inputs, sequence_length=None, initial_state=None, dtype=None, parallel_iterations=None, swap_memory=False, time_major=False, scope=None)
```
并且可能会非常犹豫到底选用哪个函数。

首先，要明确一点，这两个函数的功能是一样的。只不过dynamic_rnn函数可以将输入完全的动态展开(fully dynamic unrolling of inputs)。

对于rnn函数，rnn是静态的展开的，可是支持dynamic computation。
`dynamic computation`是指图是展开到`max_sequence_length`的，可是如果在参数中提供`sequence_length`的话，那么使用条件控制计算只会计算到`sequence_length`，这样也许可以节省时间。

`dynamic unrolling of inputs`则是不同的含义，它是指图根据`sequence_length`的长度来进行展开。

对于rnn，为了在`max_sequence_length`处获得正确的状态，通常是给输入的左边补零右对齐，这样的话，它的计算就是从`len(inputs)-max(sequence_length)`处开始的。