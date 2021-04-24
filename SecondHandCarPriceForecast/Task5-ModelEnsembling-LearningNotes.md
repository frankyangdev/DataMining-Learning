### 1. Notebook ###

运行后的Notebook: [T5-ModelEnsemble.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T5-ModelEnsemble.ipynb)

### 2. 融合代码学习 ###


如果你不能做选择的话，你可以同时选择这2种，使用stacked泛化器创建stacked集成和折外预测。然后使用留出集在第三层进一步结合这些stacked模型。

Stacking with logistic regression

使用逻辑斯谛回归做融合是一个非常经典的stacking方法。

当创建一个用于预测的测试集时，你可以一次性完成该操作，或者利用折外估计的模型(out-of-fold predictors)完成。当然为了减少模型和代码的复杂性，更倾向于一次性完成。

一切皆为超参数(hyper-parameter)

当我们使用stacking/blending/meta-modeling时，一个良好的想法就是所有的行为都是融合模型的参数。

比如说：

* 不标准化数据
* 使用z标准化
* 使用0-1标准化
这些都是可以去调从而提高集成的效果。同样的，使用多少个基模型的数量也是可以去调整优化的。特征选择（前70%）或数据填补（缺失填补）也是一系列的参数。

使用随机网格搜索就是一个比较好的调参方法，它在调整这些参数的时候确实是有起到作用的。




### Reference ###

1. [模型融合(stacking & blending) 之通过多个kaggle竞赛分析模型融合的方法和效果](https://blog.csdn.net/m0_37870649/article/details/104409760)
