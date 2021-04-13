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


**XGB和GBDT区别：**

* • XGBoost的基学习器除了可以是CART（这个时候就是GBDT）也可以是线性分类器，而GBDT只能是CART。
* • XGBoost在代价函数中加入了正则项，用于控制模型的复杂度(正则项的方式不同，如果你仔细点话，GBDT是一种类似于缩         减系数，而XGBoost类似于L2正则化项)。
* • XGBoost借鉴了随机森林的做法，支持特征抽样，不仅防止过拟合，还能减少计算
* • XGBoost工具支持并行化
*　• 综合来说Xgboost的运算速度和算法精度都会优于GBDT

**XGB和LGB区别：**

1、直方图优化，对连续特征进行分桶，在损失了一定精度的情况下大大提升了运行速度，并且在gbm的框架下，基学习器的“不精确”分箱反而增强了整体的泛化性能；

2、goss 树的引入；

3、efb，对稀疏特征做了“捆绑”的优化功能；

4、直接支持对于类别特征进行训练（实际上内部是对类别特征做了类似编码的操作了）• XGBoost的基学习器除了可以是CART（这个时候就是GBDT）也可以是线性分类器，而GBDT只能是CART。
      • XGBoost在代价函数中加入了正则项，用于控制模型的复杂度(正则项的方式不同，如果你仔细点话，GBDT是一种类似于缩         减系数，而XGBoost类似于L2正则化项)。
      • XGBoost借鉴了随机森林的做法，支持特征抽样，不仅防止过拟合，还能减少计算
      • XGBoost工具支持并行化
      • 综合来说Xgboost的运算速度和算法精度都会优于GBDT

5、树的生长方式由level-wise变成leaf-wise










