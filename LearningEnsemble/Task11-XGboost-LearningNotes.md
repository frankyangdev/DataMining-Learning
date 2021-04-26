### Notebook ###

已运行Notebook [Task11-XGBoost.ipynb](Task11-XGBoost.ipynb)



### XGBoost的核心算法 ###

* 不断地添加树，不断地进行特征分裂来生长一棵树，每次添加一个树，其实是学习一个新函数f(x)，去拟合上次预测的残差。
* 当我们训练完成得到k棵树，我们要预测一个样本的分数，其实就是根据这个样本的特征，在每棵树中会落到对应的一个叶子节点，每个叶子节点就对应一个分数
最后只需要将每棵树对应的分数加起来就是该样本的预测值。


### XGBoost与GBDT有什么不同 ###
除了算法上与传统的GBDT有一些不同外，XGBoost还在工程实现上做了大量的优化。总的来说，两者之间的区别和联系可以总结成以下几个方面。

* GBDT是机器学习算法，XGBoost是该算法的工程实现。
* 在使用CART作为基分类器时，XGBoost显式地加入了正则项来控制模 型的复杂度，有利于防止过拟合，从而提高模型的泛化能力。
* GBDT在模型训练时只使用了代价函数的一阶导数信息，XGBoost对代 价函数进行二阶泰勒展开，可以同时使用一阶和二阶导数。
* 传统的GBDT采用CART作为基分类器，XGBoost支持多种类型的基分类 器，比如线性分类器。
* 传统的GBDT在每轮迭代时使用全部的数据，XGBoost则采用了与随机 森林相似的策略，支持对数据进行采样。
* 传统的GBDT没有设计对缺失值进行处理，XGBoost能够自动学习出缺 失值的处理策略。































### Reference ###
1. [LightGBM调参笔记](https://blog.csdn.net/u012735708/article/details/83749703)
2. [机器学习集成学习之XGBoost（基于python实现）](https://zhuanlan.zhihu.com/p/143009353)
3. [XGBoost算法](https://blog.csdn.net/weixin_41510260/article/details/95339201)
4. [通俗理解kaggle比赛大杀器xgboost](https://blog.csdn.net/v_JULY_v/article/details/81410574)
