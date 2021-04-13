### 1.  Notebook：

运行结果：

[T1-Understanding.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T1-Understanding.ipynb)

[T1-SecondHandCar-Baseline.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T1-SecondHandCar-Baseline.ipynb)


### 2. Code Study

**2.1 Gradient boosting Decision Tree(GBDT)**

GBDT是GB和DT的结合。要注意的是这里的决策树是分类回归树（是一种二叉树），GBDT中的决策树是个弱模型，有些GBDT的实现加入了随机抽样（subsample 0.5<=f <=0.8）提高模型的泛化能力。通过交叉验证的方法选择最优的参数。


**2.2 Xgboost算法**

* 全称：eXtreme Gradient Boosting（极值梯度提升算法）
* 作者：陈天奇(华盛顿大学博士) 
* 基础：GBDT 
* 所属：boosting迭代型、树类算法。 
* 适用范围：分类、回归等
* 优点：速度快、效果好、能处理大规模数据、支持多种语言、支持自定义损失函数等等。

**2.3 LightGBM** 

LightGBM是个快速、分布式的、高性能的基于决策树算法的梯度提升框架。可用于排序、分类、回归以及很多其他的机器学习任务中。

因为LightGBM是基于决策树算法的，它采用最优的leaf-wise策略分裂叶子节点，然而其它的提升算法分裂树一般采用的是level-wise。因此，在LightGBM算法中，当增长到相同的叶子节点，leaf-wise算法比level-wise算法减少更多的loss，因此导致更高的精度。与此同时，它的速度也让人感到震惊，这就是该算法名字Light的原因。


**XGB和GBDT区别：**

*　XGBoost的基学习器除了可以是CART（这个时候就是GBDT）也可以是线性分类器，而GBDT只能是CART。
* XGBoost在代价函数中加入了正则项，用于控制模型的复杂度(正则项的方式不同，如果你仔细点话，GBDT是一种类似于缩         减系数，而XGBoost类似于L2正则化项)。
* XGBoost借鉴了随机森林的做法，支持特征抽样，不仅防止过拟合，还能减少计算
* XGBoost工具支持并行化
*　综合来说Xgboost的运算速度和算法精度都会优于GBDT

**XGB和LGB区别：**

＊　直方图优化，对连续特征进行分桶，在损失了一定精度的情况下大大提升了运行速度，并且在gbm的框架下，基学习器的“不精确”分箱反而增强了整体的泛化性能；

＊　goss 树的引入；

＊　efb，对稀疏特征做了“捆绑”的优化功能；

＊　直接支持对于类别特征进行训练（实际上内部是对类别特征做了类似编码的操作了）• XGBoost的基学习器除了可以是CART（这个时候就是GBDT）也可以是线性分类器，而GBDT只能是CART。
      • XGBoost在代价函数中加入了正则项，用于控制模型的复杂度(正则项的方式不同，如果你仔细点话，GBDT是一种类似于缩         减系数，而XGBoost类似于L2正则化项)。
      • XGBoost借鉴了随机森林的做法，支持特征抽样，不仅防止过拟合，还能减少计算
      • XGBoost工具支持并行化
      • 综合来说Xgboost的运算速度和算法精度都会优于GBDT

＊　树的生长方式由level-wise变成leaf-wise



Reference:
[机器学习算法（15）之Xgboost算法](ttps://blog.csdn.net/qq_20412595/article/details/82621744)

[LGB算法梳理](https://blog.csdn.net/huochangu8606/article/details/99655759)









