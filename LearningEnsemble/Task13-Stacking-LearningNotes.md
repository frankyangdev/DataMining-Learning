### Notebook ###

已运行Notebook: [Task12-Blending and Stacking.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/LearningEnsemble/Task12-Blending%20and%20Stacking.ipynb)

### Stacking ###

* Stacking是一种多层模型，将已训练好的多个模型作为基分类器。然后将这几个学习器的预测结果作为新的训练集，来学习一个新的学习器。
* 即可以看成是一种结合策略，使用另外一个机器学习算法来将个体机器学习器的结果结合在一起。
* 我们称第一层学习器为初级学习器，称第二层学习器为次级学习器。
* 通常情况下，为了防止过拟合，次级学习器宜选用简单模型。如在回归问题中，可以使用线性回归；在分类问题中，可以使用logistic。

stacking模型调参包括对基模型和元模型进行调参。对于基模型，因为我们在生成元特征的时候要使用相同的K折划分，所以我们使用交叉验证+网格搜索来调参时最好使用与生成元特征相同的Kfold。对于元模型的调参，使用交叉验证+网格搜索来调参时，为了降低过拟合的风险，我们最好也使用与元特征生成时同样的Kfold。

 综上，stacking方法从一开始就得确定一个Kfold，这个Kfold将伴随对基模型的调参、生成元特征以及对元模型的调参，贯穿整个stacking流程。当然，由于我们生成基模型时未使用全部数据，我们可以使用多个不同的Kfold来生成多个stacking模型然后进行加权，这样可以进一步提高算法的鲁棒性。

 另外，基模型的选择需要考虑的是：基模型之间的相关性要尽量小，同时基模型之间的性能表现不能差距太大。













### Reference ###
1. [模型集成之stacking](https://blog.csdn.net/xiaoliuzz/article/details/79298841)

