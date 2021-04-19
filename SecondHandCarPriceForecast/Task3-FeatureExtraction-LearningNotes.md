### 1. Notebook ###

运行结果： [T3-FeaturesExtraction.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T3-FeaturesExtraction.ipynb)

### 2. 箱线图(Box plot) ###

箱线图（Box plot）也称箱须图（Box-whisker Plot）、箱线图、盒图，可以用来反映一组或多组连续型定量数据分布的中心位置和散布范围，因形状如箱子而得名。1977年，美国著名数学家John W. Tukey首先在他的著作《Exploratory Data Analysis》中介绍了箱形图。

* 连续型数据：在一定区间内可以任意取值的变量叫连续变量，其数值是连续不断的。例如，生产零件的规格尺寸，人体测量的身高、体重等，其数值只能用测量或计量的方法取得。可视化这类数据的图表主要有箱形图和直方图。

* 离散型数据：数值只能用自然数或整数单位计算的则为离散变量。例如，企业个数，职工人数，设备台数等，只能按计量单位数计数，数值一般用计数方法取得。大多数图表可视化的都是这类数据，比如柱状图、折线图等。

![image](https://user-images.githubusercontent.com/39177230/115207784-7b0dc680-a12e-11eb-8ab8-a24e6e60a7fe.png)


在箱线图中，箱子的中间有一条线，代表了数据的中位数。箱子的上下底，分别是数据的上四分位数（Q3）和下四分位数（Q1），这意味着箱体包含了50%的数据。因此，箱子的高度在一定程度上反映了数据的波动程度。上下边缘则代表了该组数据的最大值和最小值。有时候箱子外部会有一些点，可以理解为数据中的“异常值”。

由于箱线图不像柱状图、折线图那样简单常见，许多人都对它敬而远之。但只要我们搞清楚了以下几个统计学的基本概念，箱线图也可以变得“平易近人”。

* 四分位数

一组数据按照从小到大顺序排列后，把该组数据四等分的数，称为四分位数。第一四分位数 (Q1)、第二四分位数 (Q2，也叫“中位数”)和第三四分位数 (Q3)分别等于该样本中所有数值由小到大排列后第25%、第50%和第75%的数字。第三四分位数与第一四分位数的差距又称四分位距（interquartile range, IQR）。

* 偏态

与正态分布相对，指的是非对称分布的偏斜状态。在统计学上，众数和平均数之差可作为分配偏态的指标之一：如平均数大于众数，称为正偏态（或右偏态）；相反，则称为负偏态（或左偏态）。

* 直观明了地识别数据批中的异常值

箱形图可以用来观察数据整体的分布情况，利用中位数，25/%分位数，75/%分位数，上边界，下边界等统计量来来描述数据的整体分布情况。通过计算这些统计量，生成一个箱体图，箱体包含了大部分的正常数据，而在箱体上边界和下边界之外的，就是异常数据。

* 判断数据的偏态和尾重

对于标准正态分布的大样本，中位数位于上下四分位数的中央，箱形图的方盒关于中位线对称。中位数越偏离上下四分位数的中心位置，分布偏态性越强。异常值集中在较大值一侧，则分布呈现右偏态；异常值集中在较小值一侧，则分布呈现左偏态。

* 比较多批数据的形状

箱子的上下限，分别是数据的上四分位数和下四分位数。这意味着箱子包含了50%的数据。因此，箱子的宽度在一定程度上反映了数据的波动程度。箱体越扁说明数据越集中，端线（也就是“须”）越短也说明数据集中。

```python
import matplotlib.pyplot as plt
import numpy as np

# Random test data
np.random.seed(19680801)
all_data = [np.random.normal(0, std, size=100) for std in range(1, 4)]
labels = ['x1', 'x2', 'x3']

fig, (ax1, ax2) = plt.subplots(nrows=1, ncols=2, figsize=(9, 4))

# rectangular box plot
bplot1 = ax1.boxplot(all_data,
                     vert=True,  # vertical box alignment
                     patch_artist=True,  # fill with color
                     labels=labels)  # will be used to label x-ticks
ax1.set_title('Rectangular box plot')

# notch shape box plot
bplot2 = ax2.boxplot(all_data,
                     notch=True,  # notch shape
                     vert=True,  # vertical box alignment
                     patch_artist=True,  # fill with color
                     labels=labels)  # will be used to label x-ticks
ax2.set_title('Notched box plot')

# fill with colors
colors = ['pink', 'lightblue', 'lightgreen']
for bplot in (bplot1, bplot2):
    for patch, color in zip(bplot['boxes'], colors):
        patch.set_facecolor(color)

# adding horizontal grid lines
for ax in [ax1, ax2]:
    ax.yaxis.grid(True)
    ax.set_xlabel('Three separate samples')
    ax.set_ylabel('Observed values')

plt.show()

```
![image](https://user-images.githubusercontent.com/39177230/115208688-6a118500-a12f-11eb-9de5-529fd8cc8c7d.png)

### 2. 数据分桶 ####

数据分桶是一种数据预处理技术，用于减少次要观察误差的影响，是一种将多个连续值分组为较少数量的“桶”的方法。

分桶的数据不一定必须是数字，它们可以是任意类型的值，如“猫”、“狗”等。分桶也可用于图像处理，通过将相邻像素组合成单个像素，可用于减少数据量。
一般在建立分类模型时，需要对连续变量离散化，特征离散化后，模型会更稳定，降低了模型过拟合的风险。比如在建立申请评分卡模型时用logistic作为基模型就需要对连续变量进行离散化，离散化通常采用分桶法。

**为什么要进行数据分桶？**

* 离散后稀疏向量内积乘法运算速度更快，计算结果也方便存储，容易扩展；
* 离散后的特征对异常值更具鲁棒性，如 age>30 为 1 否则为 0，对于年龄为 200 的也不会对模型造成很大的干扰；
* LR 属于广义线性模型，表达能力有限，经过离散化后，每个变量有单独的权重，这相当于引入了非线性，能够提升模型的表达能力，加大拟合；
* 离散后特征可以进行特征交叉，提升表达能力，由 M+N 个变量编程 M*N 个变量，进一步引入非线形，提升了表达能力；
* 特征离散后模型更稳定，如用户年龄区间，不会因为用户年龄长了一岁就变化；
* 可以将缺失作为独立的一类带入模型；
* 将所有的变量变换到相似的尺度上。


### 3. sklearn.feature_selection.SequentialFeatureSelector ###

`class sklearn.feature_selection.SequentialFeatureSelector(estimator, *, n_features_to_select=None, direction='forward', scoring=None, cv=5, n_jobs=None)`

Transformer that performs Sequential Feature Selection.

This Sequential Feature Selector adds (forward selection) or removes (backward selection) features to form a feature subset in a greedy fashion. At each stage, this estimator chooses the best feature to add or remove based on the cross-validation score of an estimator.

**Example**

```python
Examples

>>>
>>> from sklearn.feature_selection import SequentialFeatureSelector
>>> from sklearn.neighbors import KNeighborsClassifier
>>> from sklearn.datasets import load_iris
>>> X, y = load_iris(return_X_y=True)
>>> knn = KNeighborsClassifier(n_neighbors=3)
>>> sfs = SequentialFeatureSelector(knn, n_features_to_select=3)
>>> sfs.fit(X, y)
SequentialFeatureSelector(estimator=KNeighborsClassifier(n_neighbors=3),
                          n_features_to_select=3)
>>> sfs.get_support()
array([ True, False,  True,  True])
>>> sfs.transform(X).shape
(150, 3)
```

### 4. [from mlxtend.feature_selection import SequentialFeatureSelector](http://rasbt.github.io/mlxtend/user_guide/feature_selection/SequentialFeatureSelector/)  ###


Sequential feature selection algorithms are a family of greedy search algorithms that are used to reduce an initial d-dimensional feature space to a k-dimensional feature subspace where k < d. The motivation behind feature selection algorithms is to automatically select a subset of features that is most relevant to the problem. The goal of feature selection is two-fold: We want to improve the computational efficiency and reduce the generalization error of the model by removing irrelevant features or noise. A wrapper approach such as sequential feature selection is especially useful if embedded feature selection -- for example, a regularization penalty like LASSO -- is not applicable.

**SequentialFeatureSelector:**

1. Sequential Forward Selection (SFS)
2. Sequential Backward Selection (SBS)
3. Sequential Forward Floating Selection (SFFS)
4. Sequential Backward Floating Selection (SBFS)


**Example**

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.datasets import load_iris

iris = load_iris()
X = iris.data
y = iris.target
knn = KNeighborsClassifier(n_neighbors=4)
```

We start by selection the "best" 3 features from the Iris dataset via Sequential Forward Selection (SFS). Here, we set forward=True and floating=False. By choosing cv=0, we don't perform any cross-validation, therefore, the performance (here: 'accuracy') is computed entirely on the training set.

```python
from mlxtend.feature_selection import SequentialFeatureSelector as SFS

sfs1 = SFS(knn, 
           k_features=3, 
           forward=True, 
           floating=False, 
           verbose=2,
           scoring='accuracy',
           cv=0)

sfs1 = sfs1.fit(X, y)
```


### Reference: ###

1. [箱线图](https://blog.csdn.net/symoriaty/article/details/93978817)
2. [特征工程 -- 数据分桶](https://blog.csdn.net/weixin_42843143/article/details/105135560)






