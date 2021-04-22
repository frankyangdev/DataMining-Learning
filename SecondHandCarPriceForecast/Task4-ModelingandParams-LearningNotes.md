### 1.Notebook ###

运行结果 ： [T4-ModelingandParams.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T4-ModelingandParams.ipynb)

### 交叉验证(Cross validation) ###

在模型建立中，通常有两个数据集：训练集（train）和测试集（test）。训练集用来训练模型；测试集是完全不参与训练的数据，仅仅用来观测测试效果的数据。
一般情况下，训练的结果对于训练集的拟合程度通常还是挺好的，但是在测试集总的表现却可能不行。比如下面的例子：

![image](https://user-images.githubusercontent.com/39177230/115723986-1a42f000-a3b3-11eb-85f5-49852c0ac824.png)

* 图一的模型是一条线型方程。 可以看到，所有的红点都不在蓝线上，所以导致了错误率很高，这是典型的不拟合的情况
* 图二 的蓝线则更加贴近实际的红点，虽然没有完全重合，但是可以看出模型表示的关系是正确的。
* 图三，所有点都在蓝线上，这时候模型计算出的错误率很低，（甚至将噪音都考虑进去了）。这个模型只在训练集中表现很好，在测试集中的表现就不行。 这是典型的‘过拟合’情况。

**交叉验证:** 就是在训练集中选一部分样本用于测试模型。保留一部分的训练集数据作为验证集/评估集，对训练集生成的参数进行测试，相对客观的判断这些参数对训练集之外的数据的符合程度。

### 留一验证（LOOCV，Leave one out cross validation ）###

只从可用的数据集中保留一个数据点，并根据其余数据训练模型。此过程对每个数据点进行迭代，比如有n个数据点，就要重复交叉验证n次。例如下图，一共10个数据，就交叉验证十次

![image](https://user-images.githubusercontent.com/39177230/115724595-ac4af880-a3b3-11eb-95d3-2aabc1cc0781.png)

**优点:**

* 适合小样本数据集
* 利用所有的数据点，因此偏差将很低

**缺点:**

* 重复交叉验证过程n次导致更高的执行时间
* 测试模型有效性的变化大。因为针对一个数据点进行测试，模型的估计值受到数据点的很大影响。如果数据点被证明是一个离群值，它可能导致更大的变化

LOOCC是保留一个数据点，同样的你也可以保留P个数据点作为验证集，这种方法叫LPOCV(Leave P Out Cross Validation)

### 验证集方法 ###

* 保留一个样本数据集， （取出训练集中20%的样本不用）
* 使用数据集的剩余部分训练模型 （使用另外的80%样本训练模型）
* 使用验证集的保留样本。（完成模型后，在20%的样本中测试）
* 如果模型在验证数据上提供了一个肯定的结果，那么继续使用当前的模型。

**优点：**

简单方便。直接将训练集按比例拆分成训练集和验证集，比如50:50。

**缺点：**

没有充分利用数据， 结果具有偶然性。如果按50:50分，会损失掉另外50%的数据信息，因为我们没有利用着50%的数据来训练模型。

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression

#已经导入数据
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=20, shuffle=True)

# Logistic Regression
model = LogisticRegression()
model.fit(X_train, y_train)
prediction = model.predict(X_test)
print('The accuracy of the Logistic Regression is: {0}'.format(metrics.accuracy_score(prediction,y_test)))

```

### K折交叉验证（k-fold cross validation） ###

针对上面通过train_test_split划分，从而进行模型评估方式存在的弊端，提出Cross Validation 交叉验证。

Cross Validation：简言之，就是进行多次train_test_split划分；每次划分时，在不同的数据集上进行训练、测试评估，从而得出一个评价结果；如果是5折交叉验证，意思就是在原始数据集上，进行5次划分，每次划分进行一次训练、评估，最后得到5次划分后的评估结果，一般在这几次评估结果上取平均得到最后的 评分。k-fold cross-validation ，其中，k一般取5或10。


![image](https://user-images.githubusercontent.com/39177230/115725970-e072e900-a3b4-11eb-9171-600fa42a5f0d.png)


训练模型需要在大量的数据集基础上，否则就不能够识别数据中的趋势，导致错误产生
同样需要适量的验证数据点。 验证集太小容易导致误差
多次训练和验证模型。需要改变训练集和验证集的划分，有助于验证模型。
步骤：
随机将整个数据集分成k折；
如图中所示，依次取每一折的数据集作验证集，剩余部分作为训练集
算出每一折测试的错误率
取这里K次的记录平均值 作为最终结果

**优点:**

* 适合大样本的数据集
* 经过多次划分，大大降低了结果的偶然性，从而提高了模型的准确性。
* 对数据的使用效率更高。train_test_split，默认训练集、测试集比例为3:1。如果是5折交叉验证，训练集比测试集为4:1；10折交叉验证训练集比测试集为9:1。数据量越大，模型准确率越高。

**缺点：**

* 对数据随机均等划分，不适合包含不同类别的数据集。比如：数据集有5类数据（ABCDE各占20%），抽取出来的也正好是按照类别划分的5类，第一折全是A，第二折全是B……这样就会导致，模型学习到测试集中数据的特点，用BCDE训练的模型去测试A类数据、ACDE的模型测试B类数据，这样准确率就会很低。

***如何确定K值？**

* 一般情况下3、5是默认选项，常建议用K=10。
* k值越低，就越有偏差；K值越高偏差就越小，但是会受到很大的变化。
* k值越小，就越类似于验证集方法；而k值越大，则越接近LOOCV方法。

### 分层交叉验证 （Stratified k-fold cross validation）###

分层是重新将数据排列组合，使得每一折都能比较好地代表整体。

标准交叉验证和分层交叉验证的区别：

* 标准交叉验证（即K折交叉验证）：直接将数据分成几折；
* 分层交叉验证：先将数据分类（class1,2,3)，然后在每个类别中划分三折。

![image](https://user-images.githubusercontent.com/39177230/115726568-6abb4d00-a3b5-11eb-9ba2-5198ba64a97d.png)


### 时间序列的交叉验证(Cross Validation for time series）###


为了解决时间序列的预测问题，可以尝试时间序列交叉验证：采用正向链接的策略，即按照时间顺序划分每一折的数据集。

从一个最小的训练集开始（这个训练集具有拟合模型所需的最少观测数）逐步地，每次都会更换训练集和测试集。在大多数情况下，不必一个个点向前移动，可以设置一次跨5个点/10个点。在回归问题中，可以使用以下代码执行交叉验证。

```python
from sklearn.model_selection import TimeSeriesSplit
X = np.array([[1, 2], [3, 4], [1, 2], [3, 4]])
y = np.array([1, 2, 3, 4])
tscv = TimeSeriesSplit(n_splits=3)

for train_index, test_index in tscv.split(X):
     print("Train:", train_index, "Validation:", val_index)
     X_train, X_test = X[train_index], X[val_index]
     y_train, y_test = y[train_index], y[val_index]

TRAIN: [0] TEST: [1]
TRAIN: [0 1] TEST: [2]
TRAIN: [0 1 2] TEST: [3]
```









### Reference ###

1. [交叉验证方法汇总](https://blog.csdn.net/WHYbeHERE/article/details/108192957)
