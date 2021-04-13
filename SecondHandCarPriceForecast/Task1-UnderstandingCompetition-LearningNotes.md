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

**Histogram VS pre-sorted（如何寻找最优切分点）**


**Pre-sorted：**

之前的GBDT工具都是基于预排序(pre-sorted)的方法（如 xgboost）。这种构造决策树的算法基本思想是：

首先，对所有特征都按照特征的数值进行预排序。

其次，在遍历分割点的时候用O(#data)的代价找到一个特征上的最好分割点。

最后，找到一个特征的分割点后，将数据分裂成左右子节点。

优点：能精确地找到分割点；缺点：空间消耗大，时间开销也大，对cache优化不友好。

**Histogram：**

直方图算法的基本思想是先把连续的浮点特征值离散化成k个整数，同时构造一个宽度为k的直方图。在遍历数据的时候，根据离散化后的值作为索引在直方图中积累统计量，当遍历一次数据后，直方图累计了需要的统计量，然后根据直方图的离散值，遍历寻找最优的分割点。

优点：内存消耗的降低，可以降到原来的1/8；计算代价的降低，预排序算法每遍历一个特征值就需要计算一次分裂的增益，而直方图只需要计算k次(k可以认为是常数)，时间复杂度从O（#data∗ *∗#feature）优化到O（k*#features）。

Histogram（直方图）做差加速，一个叶子的直方图可以由它的父亲节点与它的兄弟的直方图做差得到。这样速度可以提升一倍。


**leaf-wise VS level-wise（决策树生长策略） **

* level-wise

大多数GBDT工具使用按层生长（level-wise）的决策树生长策略。Level-wise过一次数据可以同时分裂同一层的叶子，容易进行多线程优化，也好控制模型复杂度，不容易过拟合。但实际上Level-wise是一种低效的算法，因为它不加区分的对待同一层的叶子，带来了很多没必要的开销，因为实际上很多叶子的分裂增益较低，没必要进行搜索和分裂。


* leaf-wise

Leaf-wise是一种更为高效的策略，每次从当前所有叶子中，找到分裂增益最大的一个叶子，然后分裂，如此循环。因此同Level-wise相比，在分裂次数相同的情况下，Leaf-wise可以降低更多的误差，得到更好的精度。Leaf-wise的缺点是可能会长出比较深的决策树，产生过拟合。因此LightGBM在Leaf-wise之上增加了一个最大深度的限制，在保证高效率的同时防止过拟合。


**特征并行和数据并行**


**特征并行：**适用于数据少，但是特征比较多的情况。LightGBM的不同worker在不同的特征子集合上分别寻找最优的分割点，然后同步找到全局最佳切分点。


**数据并行：**适用于数据量大，特征比较少的情况。数据并行对数据进行水平切分，每个worker上的数据先建立起局部直方图，然后合并成全局的直方图，之后找到最优的分割点。


**投票并行：**适用于数据量大，特征也大的情况。投票并行主要是对数据并行的一个优化。每个worker首先会找到它们数据中最优的一些特征，然后进行全局的投票，根据投票的结果，选择top的特征的直方图进行合并，再寻求全局的最优分割点。




Reference:
[机器学习算法（15）之Xgboost算法](ttps://blog.csdn.net/qq_20412595/article/details/82621744)

[LGB算法梳理](https://blog.csdn.net/huochangu8606/article/details/99655759)









