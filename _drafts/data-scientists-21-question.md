### Q1.解释一下什么是正则化以及它为什么如此有用?

正则化是一种控制模型复杂度和泛化程度一个很好的手段。对于一个模型，过高的模型复杂度意味着其泛化程度较差，当遇到新的数据时，预测效果不好；正则化后，相当于降低了模型的方差，可以有效控制模型复杂度。以L2正则化为例，如果没有正则项，那么目标函数的目标就是单一地提高在训练集上的精确度，而训练集并非是真实数据的分布完全一致，少量异常点的出现，将会导致模型的剧烈抖动，因而对于新来的数据预测效果很差，加上正则项之后，则让模型对新来的数据依然有很好的预测效果。


### Q2.你最欣赏那个数据科学家？哪家创业公司？


### Q3.当你使用多元回归来产生一个定量输出的预测模型时，你会如何来验证你的模型？



### Q4.解释下召回率和精度，它们和ROC曲线的关系是什么样的？

召回率是预测对的正例数占所有正例数的比率；而精度是指预测对的正例数占预测的所有的正例数的比率。
FPR（错误的判断为阳性之比率）-ROC横轴
TPR（正确的判断为阳性之比率）-ROC 纵轴

### Q5.你如何证明你对一个算法的改善确实一种提高而不是无效的？
看其在测试集上的效果。

###Q6.

### Q7.你对price optimization, price elasticity, inventory management, competitive intelligence熟悉吗？举出一些例子。

### Q8.什么是statistical power?

### Q9.解释下重采样方法的概念以及它为什么有效和它的局限性。



### Q10.产生太多假阳性或者假阴性是否更好呢？

这要取决于你的应用场景，不同的应用场景对不同的误分所产生的代价是不一样；举例说来，


### Q11.selection bias是什么？为什么它如此重要，解释下你将如何避免它？


### Q12.举一个你使用实验设计来回答关于用户行为的问题的例子。


### Q13.解释下“long”和“wide”格式数据的区别。



### Q14.



### Q15.解释下Edward Tuffe的“chart junk”的概念。



### Q16.你将如何寻找异常点？你找到后将如何处理?



### Q17.你将如何使用要么极值理论，蒙特卡洛或者数理统计来正确的估计一个稀有事件发生的概率?


### Q18.什么是推荐引擎？它工作的原理是什么？


### Q19.解释下什么是假阳性和假阴性。为什么把它们彼此分开如此重要？


### Q20.你使用什么工具可视化？你认为Tableau怎么样? R  ? SAS ? 如何在一个图表或者视频中有效的表示5维的数据？


### Bonus Question： 解释下什么是过拟合，你将如何控制过拟合？

