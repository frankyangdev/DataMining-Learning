### Bagging ###


**Bagging** (short for **bootstrap aggregating**) is a technique for reducing generalization
error by combining several models (Breiman, 1994). The idea is to
train several different models separately, then have all of the models vote on the
output for test examples. This is an example of a general strategy in machine
learning called **model averaging**. Techniques employing this strategy are known
as ensemble methods.

The reason that model averaging works is that different models will usually
not make all the same errors on the test set.


![image](https://user-images.githubusercontent.com/39177230/115105133-81197100-9f8f-11eb-9e13-07177ddac74c.png)

A cartoon depiction of how bagging works. Suppose we train an 8 detector on
the dataset depicted above, containing an 8, a 6 and a 9. Suppose we make two different
resampled datasets. The bagging training procedure is to construct each of these datasets
by sampling with replacement. 

The first dataset omits the 9 and repeats the 8. On this dataset, the detector learns that a loop on top of the digit corresponds to an 8. 
On the second dataset, we repeat the 9 and omit the 6. In this case, the detector learns that a loop on the bottom of the digit corresponds to an 8. 
Each of these individual classification rules is brittle, but if we average their output then the detector is robust,achieving maximal confidence only when both loops of the 8 are present.

Bagging is a method that allows the same kind of model, training algorithm and objective function to be reused several times.


### Bagging（bootstrap aggregation）###

Bootstraping的名称来自于成语 ‘’pull up by your own bootstraps‘’， 意思是依靠你自己的资源，称为自助法，它是一种有放回的抽样方法。Bootstrap的本意是指高靴子后面的悬挂物，小环，是穿鞋子时用手向上拉的工具。意思是不可能的事情后来意思发生了转变，隐喻 ‘’不需要外界帮助，仅依靠自身力量让自己变得更好‘’。


### Bagging策略 ###

对数据进行自助采样法，对结果进行简单投票法。 对于给定的包含m个样本的数据集，我们随机选择一个样本放入采样集中，再把该样本放回初始数据集，使得下次采样仍有可能被选中。我们这样选择的样本有的在采样集里面重复出现，有的则从未出现。我们分类任务使用简单投票法；对分类任务使用简单平均法；若分类投票出现相同的票数情况，则随机选择一个。

![image](https://user-images.githubusercontent.com/39177230/115105580-76141000-9f92-11eb-9b54-e94af9aa4f6e.png)

#### 随机森林(Random Forest，简称RF) ####
随机森林是Bagging的一个扩展变体，RF在以决策树为基学习器构建Bagging集成的基础上，进一步在决策树的训练过程中映入了随机属性选择。具体来说，传统的决策树在选择划分属性时在当前节点选择一个最优属性；而在RF中对基决策树的每个节点，先从该节点的属性集合中随机选择一个包含k个属性的子集，然后再从这个子集中选择一个最优属性用于划分。在很多例子中表现功能强大，进一步使泛化性能提升，被称为 ‘代表集成学习技术水平的方法’。


#### 随机森林算法概述 ####

随机森林算法是上世纪八十年代Breiman等人提出来的，其基本思想就是构造很多棵决策树，形成一个森林，然后用这些决策树共同决策输出类别是什么。随机森林算法及在构建单一决策树的基础上的，同时是单一决策树算法的延伸和改进。在整个随机森林算法的过程中，有两个随机过程，第一个就是输入数据是随机的从整体的训练数据中选取一部分作为一棵决策树的构建，而且是有放回的选取；第二个就是每棵决策树的构建所需的特征是从整体的特征集随机的选取的，这两个随机过程使得随机森林很大程度上避免了过拟合现象的出现。

![image](https://user-images.githubusercontent.com/39177230/115106064-3ac71080-9f95-11eb-86c7-7eaadddd1bab.png)



![image](https://user-images.githubusercontent.com/39177230/115105652-defb8800-9f92-11eb-8f87-3957aca6e95c.png)

#### 随机森林在Bagging的基础上做了修改 ####

* 从样本集中用Bootstrap采样选出n个样本；
* 从所有属性中随机选择k个属性，选择最佳分割属性作为节点建立CART决策树；
* 重复以上两个步骤m次，即建立了m棵CART决策树
* 这m棵CART决策树形成随机森林，通过投票表决结果，决定数据属于哪一类

#### 随机森林、Bagging和决策树的关系 ####
可以使用决策树作为基本分类器
也可以使用SVM，Logistic回归等其他分类器，习惯上，这些分类器组成的 ‘’总分类器‘’，仍然叫做随机森林。

* 偏差：模型预测值的期望与真实值之间的差异，反应的是模型的拟合能力。
* 方差：反应的是训练集的变化所导致的学习性能的变化，即刻画了数据扰动所造成的影响，模型过拟合时会出现较大的方差。
而Bagging是对多个弱学习器求平均，这样能减少模型的方差，从而提高模型稳定性。

由于Bagging算法每次都进行采样来训练模型，因此泛化能力很强，对于降低模型的方差很有作用。Bagging适合对偏差低、方差高的模型进行融合。当然对于训练集的拟合程度就会差一些，也就是模型的偏倚会大一些。

Bagging + 决策树 = 随机森林

#### 随机森林算法的注意点 ####

1. 在构建决策树的过程中是不需要剪枝的。
2. 整个森林的树的数量和每棵树的特征需要人为进行设定。
3. 构建决策树的时候分裂节点的选择是依据最小基尼系数的。

#### 随机森林有很多的优点 ####

a. 在数据集上表现良好，两个随机性的引入，使得随机森林不容易陷入过拟合。

b. 在当前的很多数据集上，相对其他算法有着很大的优势，两个随机性的引入，使得随机森林具有很好的抗噪声能力。

c. 它能够处理很高维度（feature很多）的数据，并且不用做特征选择，对数据集的适应能力强：既能处理离散型数据，也能处理连续型数据，数据集无需规范化。

d. 在创建随机森林的时候，对generlization error使用的是无偏估计。

e. 训练速度快，可以得到变量重要性排序。

f. 在训练过程中，能够检测到feature间的互相影响。

g 容易做成并行化方法。

h. 实现比较简单。
































### Reference ###

1.  花书 Deep Learning (Ian Goodfellow ,Yoshua Bengio, and Aaron Gourville) - chapter 7.11 Bagging and Other Ensemble Methods
2. [集成算法(Bagging，随机森林)](https://blog.csdn.net/H_hei/article/details/84196235)
3. [集成学习算法总结----Boosting和Bagging](https://blog.csdn.net/a1b2c3d4123456/article/details/51834272)

