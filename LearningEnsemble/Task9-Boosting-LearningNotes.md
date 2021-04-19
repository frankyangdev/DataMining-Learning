### 1.Notebook ###

运行代码示例： []()

###


#### Decision Tree [code](https://github.com/Jack-Cherish/Machine-Learning/tree/master/Decision%20Tree)

#### Logistic [code](https://github.com/Jack-Cherish/Machine-Learning/tree/master/Logistic)

#### LightGBM [code](https://github.com/microsoft/LightGBM) [onSpark](https://github.com/Azure/mmlspark/blob/master/notebooks/samples/LightGBM%20-%20Quantile%20Regression%20for%20Drug%20Discovery.ipynb)

#### catboost [code](https://github.com/catboost/catboost)



### [逻辑回归模型](https://blog.csdn.net/han_xiaoyang/article/details/49123419)

它将数据拟合到一个logit函数(或者叫做logistic函数)中，从而能够完成对事件发生的概率进行预测。
如果线性回归的结果输出是一个连续值，而值的范围是无法限定的，那我们有没有办法把这个结果值映射为可以帮助我们判断的结果呢。而如果输出结果是 (0,1) 的一个概率值.
![image](https://user-images.githubusercontent.com/39177230/112442333-af3bd480-8d86-11eb-8fb1-a235480d4e82.png)

从函数图上可以看出，函数y=g(z)在z=0的时候取值为1/2，而随着z逐渐变小，函数值趋于0，z逐渐变大的同时函数值逐渐趋于1，而这正是一个概率的范围。

所以我们定义线性回归的预测函数为Y=WTX，那么逻辑回归的输出Y= g(WTX)，其中y=g(z)函数正是上述sigmoid函数(或者简单叫做S形函数)。

所谓的代价函数Cost Function，其实是一种衡量我们在这组参数下预估的结果和实际结果差距的函数，比如说线性回归的代价函数定义为:
![image](https://user-images.githubusercontent.com/39177230/112442759-26716880-8d87-11eb-83b5-1e036532cbf9.png)

查看数据在空间的分布 
```python
from numpy import loadtxt, where
from pylab import scatter, show, legend, xlabel, ylabel
 
#load the dataset
data = loadtxt('/home/HanXiaoyang/data/data1.txt', delimiter=',')
 
X = data[:, 0:2]
y = data[:, 2]
 
pos = where(y == 1)
neg = where(y == 0)
scatter(X[pos, 0], X[pos, 1], marker='o', c='b')
scatter(X[neg, 0], X[neg, 1], marker='x', c='r')
xlabel('Feature1/Exam 1 score')
ylabel('Feature2/Exam 2 score')
legend(['Fail', 'Pass'])
show()

```
![image](https://user-images.githubusercontent.com/39177230/112443647-2b82e780-8d88-11eb-9201-e85bf4c0d7da.png)


写好计算sigmoid函数、代价函数、和梯度下降的程序
```python
 
 def sigmoid(X):
    '''Compute sigmoid function '''
    den =1.0+ e **(-1.0* X)
    gz =1.0/ den
    return gz
def compute_cost(theta,X,y):
    '''computes cost given predicted and actual values'''
    m = X.shape[0]#number of training examples
    theta = reshape(theta,(len(theta),1))
    
    J =(1./m)*(-transpose(y).dot(log(sigmoid(X.dot(theta))))- transpose(1-y).dot(log(1-sigmoid(X.dot(theta)))))
    
    grad = transpose((1./m)*transpose(sigmoid(X.dot(theta))- y).dot(X))
    #optimize.fmin expects a single value, so cannot return grad
    return J[0][0]#,grad
def compute_grad(theta, X, y):
    '''compute gradient'''
    theta.shape =(1,3)
    grad = zeros(3)
    h = sigmoid(X.dot(theta.T))
    delta = h - y
    l = grad.size
    for i in range(l):
        sumdelta = delta.T.dot(X[:, i])
        grad[i]=(1.0/ m)* sumdelta *-1
    theta.shape =(3,)
    return  grad
```

梯度下降算法得到的结果判定边界是如下的样子:

![image](https://user-images.githubusercontent.com/39177230/112444153-b7950f00-8d88-11eb-9089-d9f8f89886cd.png)

使用我们的判定边界对training data做一个预测，然后比对一下准确率：
计算出来的结果是89.2%

```python
def predict(theta, X):
    '''Predict label using learned logistic regression parameters'''
    m, n = X.shape
    p = zeros(shape=(m,1))
    h = sigmoid(X.dot(theta.T))
    for it in range(0, h.shape[0]):
        if h[it]>0.5:
            p[it,0]=1
        else:
            p[it,0]=0
    return p
#Compute accuracy on our training set
p = predict(array(theta), it)
print'Train Accuracy: %f'%((y[where(p == y)].size / float(y.size))*100.0)
```


### [决策树模型](https://blog.csdn.net/c406495762/article/details/76262487)

分类决策树模型是一种描述对实例进行分类的树形结构。决策树由结点(node)和有向边(directed edge)组成。结点有两种类型：内部结点(internal node)和叶结点(leaf node)。内部结点表示一个特征或属性，叶结点表示一个类。

#### 决策树的一些优点：

易于理解和解释，决策树可以可视化。
几乎不需要数据预处理。其他方法经常需要数据标准化，创建虚拟变量和删除缺失值。决策树还不支持缺失值。
使用树的花费（例如预测数据）是训练数据点(data points)数量的对数。
可以同时处理数值变量和分类变量。其他方法大都适用于分析一种变量的集合。
可以处理多值输出变量问题。
使用白盒模型。如果一个情况被观察到，使用逻辑判断容易表示这种规则。相反，如果是黑盒模型（例如人工神经网络），结果会非常难解释。
即使对真实模型来说，假设无效的情况下，也可以较好的适用。

#### 决策树的一些缺点：

决策树学习可能创建一个过于复杂的树，并不能很好的预测数据。也就是过拟合。修剪机制（现在不支持），设置一个叶子节点需要的最小样本数量，或者数的最大深度，可以避免过拟合。
决策树可能是不稳定的，因为即使非常小的变异，可能会产生一颗完全不同的树。这个问题通过decision trees with an ensemble来缓解。
学习一颗最优的决策树是一个NP-完全问题under several aspects of optimality and even for simple concepts。因此，传统决策树算法基于启发式算法，例如贪婪算法，即每个节点创建最优决策。这些算法不能产生一个全家最优的决策树。对样本和特征随机抽样可以降低整体效果偏差。
概念难以学习，因为决策树没有很好的解释他们，例如，XOR, parity or multiplexer problems.
如果某些分类占优势，决策树将会创建一棵有偏差的树。因此，建议在训练之前，先抽样使样本均衡。

#### 特征选择
    特征选择在于选取对训练数据具有分类能力的特征。这样可以提高决策树学习的效率，如果利用一个特征进行分类的结果与随机分类的结果没有很大差别，则称这个特征是没有分类能力的。经验上扔掉这样的特征对决策树学习的精度影响不大。通常特征选择的标准是信息增益(information gain)或信息增益比
    
#### 香农熵
熵定义为信息的期望值。在信息论与概率统计中，熵是表示随机变量不确定性的度量。如果待分类的事务可能划分在多个分类之中，则符号xi的信息定义为

![image](https://user-images.githubusercontent.com/39177230/112446910-aa2d5400-8d8b-11eb-8d63-08caa1bb14c1.png)

![image](https://user-images.githubusercontent.com/39177230/112446950-b87b7000-8d8b-11eb-9dba-c8b09f856216.png)


### [GBDT模型](https://zhuanlan.zhihu.com/p/45145899)

梯度提升树GBDT

#### CART回归树
GBDT是一个集成模型，可以看做是很多个基模型的线性相加，其中的基模型就是CART回归树。

CART树是一个决策树模型，与普通的ID3，C4.5相比，CART树的主要特征是，他是一颗二分树，每个节点特征取值为“是”和“不是”。举个例子，在ID3中如果天气是一个特征，那么基于此的节点特征取值为“晴天”、“阴天”、“雨天”，而CART树中就是“不是晴天”与“是晴天”。

这样的决策树递归的划分每个特征，并且在输入空间的每个划分单元中确定唯一的输出

![image](https://user-images.githubusercontent.com/39177230/112447936-d5fd0980-8d8c-11eb-908e-32455914be37.png)

![image](https://user-images.githubusercontent.com/39177230/112447978-e0b79e80-8d8c-11eb-950d-8441051e0823.png)

####  GBDT模型

GBDT模型是一个集成模型，是很多CART树的线性相加。

![image](https://user-images.githubusercontent.com/39177230/112448157-13619700-8d8d-11eb-9c5d-52f6ff8912b9.png)


### [XGBoost模型](https://blog.csdn.net/wuzhongqiang/article/details/104854890)

常见的机器学习算法：

* 监督学习算法：逻辑回归，线性回归，决策树，朴素贝叶斯，K近邻，支持向量机，集成算法Adaboost等
* 无监督算法：聚类，降维，关联规则, PageRank等

根据各个弱分类器之间有无依赖关系，分为Boosting和Bagging

* Boosting流派，各分类器之间有依赖关系，必须串行，比如Adaboost、GBDT(Gradient Boosting Decision Tree)、Xgboost
* Bagging流派，各分类器之间没有依赖关系，可各自并行，比如随机森林（Random Forest）

AdaBoost，是英文"Adaptive Boosting"（自适应增强），它的自适应在于：前一个基本分类器分错的样本会得到加强，加权后的全体样本再次被用来训练下一个基本分类器。同时，在每一轮中加入一个新的弱分类器，直到达到某个预定的足够小的错误率或达到预先指定的最大迭代次数。白话的讲，就是它在训练弱分类器之前，会给每个样本一个权重，训练完了一个分类器，就会调整样本的权重，前一个分类器分错的样本权重会加大，这样后面再训练分类器的时候，就会更加注重前面分错的样本， 然后一步一步的训练出很多个弱分类器，最后，根据弱分类器的表现给它们加上权重，组合成一个强大的分类器，就足可以应付整个数据集了。 这就是AdaBoost， 它强调自适应，不断修改样本权重， 不断加入弱分类器进行boosting。

GBDT(Gradient Boost Decision Tree)就是另一种boosting的方式， 上面说到AdaBoost训练弱分类器关注的是那些被分错的样本，AdaBoost每一次训练都是为了减少错误分类的样本。 而GBDT训练弱分类器关注的是残差，也就是上一个弱分类器的表现与完美答案之间的差距，GBDT每一次训练分类器，都是为了减少这个差距

xgboost与gbdt比较大的不同就是目标函数的定义，但这俩在策略上是类似的，都是聚焦残差（更准确的说， xgboost其实是gbdt算法在工程上的一种实现方式），GBDT旨在通过不断加入新的树最快速度降低残差，而XGBoost则可以人为定义损失函数（可以是最小平方差、logistic loss function、hinge loss function或者人为定义的loss function），只需要知道该loss function对参数的一阶、二阶导数便可以进行boosting，其进一步增大了模型的泛化能力，其贪婪法寻找添加树的结构以及loss function中的损失函数与正则项等一系列策略也使得XGBoost预测更准确。

![image](https://user-images.githubusercontent.com/39177230/112449276-2de84000-8d8e-11eb-967e-0f2a7bc38309.png)

xgboost相比于GBDT有哪些优点：

* 精度更高：GBDT只用到一阶泰勒， 而xgboost对损失函数进行了二阶泰勒展开， 一方面为了增加精度， 另一方面也为了能够自定义损失函数，二阶泰勒展开可以近似大量损失函数
* 灵活性更强：GBDT以CART作为基分类器，而Xgboost不仅支持CART，还支持线性分类器，另外，Xgboost支持自定义损失函数，只要损失函数有一二阶导数。
* 正则化：xgboost在目标函数中加入了正则，用于控制模型的复杂度。有助于降低模型方差，防止过拟合。正则项里包含了树的叶子节点个数，叶子节点权重的L2范式。
* Shrinkage（缩减）：相当于学习速率。这个主要是为了削弱每棵树的影响，让后面有更大的学习空间，学习过程更加的平缓
* 列抽样：这个就是在建树的时候，不用遍历所有的特征了，可以进行抽样，一方面简化了计算，另一方面也有助于降低过拟合
* 缺失值处理：这个是xgboost的稀疏感知算法，加快了节点分裂的速度
* 并行化操作：块结构可以很好的支持并行计算



### [LightGBM模型](https://blog.csdn.net/wuzhongqiang/article/details/105350579)

xgboost寻找最优分裂点的复杂度=特征数量×分裂点的数量×样本的数量

Lightgbm里面的直方图算法就是为了减少分裂点的数量， Lightgbm里面的单边梯度抽样算法就是为了减少样本的数量， 而Lightgbm里面的互斥特征捆绑算法就是为了减少特征的数量。 并且后面两个是Lightgbm的亮点所在。

与xgboost对比一下，总结一下lightgbm的优点作为收尾， 从内存和速度两方面总结：

* 内存更小
* XGBoost 使用预排序后需要记录特征值及其对应样本的统计值的索引，而 LightGBM 使用了直方图算法将特征值转变为 bin 值，且不需要记录特征到样本的索引，将空间复杂度从 O(2*#data) 降低为 O(#bin) ，极大的减少了内存消耗；
* LightGBM 采用了直方图算法将存储特征值转变为存储 bin 值，降低了内存消耗；
* LightGBM 在训练过程中采用互斥特征捆绑算法减少了特征数量，降低了内存消耗。
* 速度更快
* LightGBM 采用了直方图算法将遍历样本转变为遍历直方图，极大的降低了时间复杂度；
* LightGBM 在训练过程中采用单边梯度算法过滤掉梯度小的样本，减少了大量的计算；
* LightGBM 采用了基于 Leaf-wise 算法的增长策略构建树，减少了很多不必要的计算量；
* LightGBM 采用优化后的特征并行、数据并行方法加速计算，当数据量非常大的时候还可以采用投票并行的策略；
* LightGBM 对缓存也进行了优化，增加了 Cache hit 的命中率。

### [Catboost模型](https://mp.weixin.qq.com/s/xloTLr5NJBgBspMQtxPoFA)

![image](https://user-images.githubusercontent.com/39177230/112450194-19f10e00-8d8f-11eb-82ff-fa4ae04ba62d.png)

CatBoost是俄罗斯的搜索巨头Yandex在2017年开源的机器学习库，是Boosting族算法的一种。CatBoost和XGBoost、LightGBM并称为GBDT的三大主流神器，都是在GBDT算法框架下的一种改进实现。XGBoost被广泛的应用于工业界，LightGBM有效的提升了GBDT的计算效率，而Yandex的CatBoost号称是比XGBoost和LightGBM在算法准确率等方面表现更为优秀的算法。

CatBoost是一种基于对称决策树（oblivious trees）为基学习器实现的参数较少、支持类别型变量和高准确性的GBDT框架，主要解决的痛点是高效合理地处理类别型特征，这一点从它的名字中可以看出来，CatBoost是由Categorical和Boosting组成。此外，CatBoost还解决了梯度偏差（Gradient Bias）以及预测偏移（Prediction shift）的问题，从而减少过拟合的发生，进而提高算法的准确性和泛化能力。

#### 与XGBoost、LightGBM相比，CatBoost的创新点有：

* 嵌入了自动将类别型特征处理为数值型特征的创新算法。首先对categorical features做一些统计，计算某个类别特征（category）出现的频率，之后加上超参数，生成新的数值型特征（numerical features）。
* Catboost还使用了组合类别特征，可以利用到特征之间的联系，这极大的丰富了特征维度。
* 采用排序提升的方法对抗训练集中的噪声点，从而避免梯度估计的偏差，进而解决预测偏移的问题。
* 采用了完全对称树作为基模型。

#### 基于GPU实现快速训练
* 密集的数值特征。 对于任何GBDT算法而言，最大的难点之一就是搜索最佳分割。尤其是对于密集的数值特征数据集来说，该步骤是建立决策树时的主要计算负担。CatBoost使用oblivious 决策树作为基模型，并将特征离散化到固定数量的箱子中以减少内存使用。就GPU内存使用而言，CatBoost至少与LightGBM一样有效。主要改进之处就是利用了一种不依赖于原子操作的直方图计算方法。
* 类别型特征。 CatBoost实现了多种处理类别型特征的方法，并使用完美哈希来存储类别型特征的值，以减少内存使用。由于GPU内存的限制，在CPU RAM中存储按位压缩的完美哈希，以及要求的数据流、重叠计算和内存等操作。通过哈希来分组观察。在每个组中，我们需要计算一些统计量的前缀和。该统计量的计算使用分段扫描GPU图元实现。
* 多GPU支持。 CatBoost中的GPU实现可支持多个GPU。分布式树学习可以通过数据或特征进行并行化。CatBoost采用多个学习数据集排列的计算方案，在训练期间计算类别型特征的统计数据。

#### CatBoost的优缺点
* 优点
性能卓越： 在性能方面可以匹敌任何先进的机器学习算法；
鲁棒性/强健性： 它减少了对很多超参数调优的需求，并降低了过度拟合的机会，这也使得模型变得更加具有通用性；
易于使用： 提供与scikit集成的Python接口，以及R和命令行界面；
实用： 可以处理类别型、数值型特征；
可扩展： 支持自定义损失函数；
* 缺点
对于类别型特征的处理需要大量的内存和时间；
不同随机数的设定对于模型预测结果有一定的影响；
