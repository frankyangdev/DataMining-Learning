### Notebook ###

已运行Notebook: [Task12-Blending and Stacking.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/LearningEnsemble/Task12-Blending%20and%20Stacking.ipynb)


### Blending vs Stacking ###

Blending 与Stacking大致相同，只是Blending的主要区别在于训练集不是通过K-Fold的CV策略来获得预测值从而生成第二阶段模型的特征，而是建立一个Holdout集。简单来说，Blending直接用不相交的数据集用于不同层的训练。

Stacking是k折交叉验证，元模型的训练数据等同于基于模型的训练数据，该方法为每个样本都生成了元特征，每生成元特征的模型不一样（k是多少，每个模型的数量就是多少）；测试集生成元特征时，需要用到k（k fold不是模型）个加权平均；


Blending的优点在于：

1.比stacking简单（因为不用进行k次的交叉验证来获得stacker feature）；
2.避开了一个信息泄露问题：generlizers和stacker使用了不一样的数据集；
3.在团队建模过程中，不需要给队友分享自己的随机种子；

而缺点在于：
1.使用了很少的数据（是划分hold-out作为测试集，并非cv）；
2.blender可能会过拟合（其实大概率是第一点导致的）；
3.stacking使用多次的CV会比较稳健。

blending是直接准备好一部分 10%留出集只在留出集上继续预测,用不相交的数据训练不同的Base Model ,将它们的输出取(加权)平均。实现简单,但对训练数据利用少了。

使用Stacking，组合1000多个模型，有时甚至要计算几十个小时；
但它也有优点：

1. 我们有可能将集成的知识迁移到到简单的分类器上；
2. 自动化的大型集成策略可以通过添加正则项有效的对抗过拟合,且并不需要太多的调参和特征选择。所以从原则上讲, stacking非常适合于那些“懒人”；
3. 这是目前提升机器学习效果最好的方法,或者说是最效率的方法human ensemble learning。




### Reference ###

1. [Stacking和Blending](https://blog.csdn.net/tomatotian/article/details/103054883)
