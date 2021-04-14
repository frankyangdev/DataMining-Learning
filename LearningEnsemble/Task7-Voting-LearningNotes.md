
### 1. Notebook ###

运行结果: [taks7-ch3.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/LearningEnsemble/taks7-ch3.ipynb)

### 2. 投票分类器(Voting Classifier) ###

**基于多分类器的结果聚合:**

![image](https://user-images.githubusercontent.com/39177230/114675012-60a4a900-9d3a-11eb-817f-beacc15f7b90.png)

![image](https://user-images.githubusercontent.com/39177230/114675085-74500f80-9d3a-11eb-910e-ebd655f520d1.png)

1. 训练多种分类器的集成学习模型:logits分类器,SVM分类器,随机森林分类器
2. 根据不同种类的分类器输出结果进行统计,将占比最大的结果作为本次投票的最终结果输出

**在数据足够大的基础上,这样可以将一系列弱学习器,通过集合的方式,将预测模型变成一个强学习器**

当用soft voting进行集成时，它不仅要看投给A多少票，B多少票，还要看到底有多大是我概率把样本分成A/B类，在使用时，就要求集合的每一个模型都能估计概率。那我们来回顾一下有哪些算法是能够估计概率的： 

* 逻辑回归
* KNN
* 决策树
* SVM（间接）

```python

# 使用scikit中自带的卫星数据
moons = make_moons(n_samples=20000)
# 逻辑回归分类器
log_clf = LogisticRegression()
#  随机森林分类器
rnd_clf = RandomForestClassifier()
# 支持向量机分类器
svm_clf = SVC(probability=True)
# 集成以上3种分类器的投票分类器

voting_clf = VotingClassifier(
    estimators=[('lr', log_clf), ('rf', rnd_clf), ('svc', svm_clf)],
    voting='soft'
)
# 分别输出 逻辑回归,随机森林,svm,投票分类器的准确率
for clf in (log_clf, rnd_clf, svm_clf, voting_clf):
    clf.fit(x_train, y_train)
    y_pred = clf.predict(x_test)
    print(clf.__class__.__name__, accuracy_score(y_test, y_pred))
    
out:
  LogisticRegression 0.891
	RandomForestClassifier 1.0
	SVC 1.0
	VotingClassifier 1.0
```

注意 voting_clf 中voting参数:

* Hard Voting Classifier：根据少数服从多数来定最终结果,独立个体的大多数类别就是改预测器的预测结果

![image](https://user-images.githubusercontent.com/39177230/114676983-6dc29780-9d3c-11eb-96e9-01e262242315.png)


* Soft Voting Classifier：将所有模型预测样本为某一类别的概率的平均值作为标准，最后横向比较,概率最高的对应的类型为最终的预测结果

![image](https://user-images.githubusercontent.com/39177230/114677050-803cd100-9d3c-11eb-8032-3d3a17637d46.png)

* Hard Voting 投票方式的弊端：如上图，最终的分类结果不是由概率值更大的模型 1 和模型 4 决定，而是由概率值相对较低的模型 2/3/5 来决定的；


### 3.各分类算法的概率计算 ###
**Soft Voting 的决策方式，要求集合的每一个模型都能估计概率；**

1. 逻辑回归算法(logistic regression)

P = σ( y_predict )

![image](https://user-images.githubusercontent.com/39177230/114677446-e590c200-9d3c-11eb-8bbd-03b9c4676b3a.png)

2. kNN 算法

* k 个样本点中，数量最多的样本所对应的类别作为最终的预测结果；
* kNN 算法也可以考虑权值，根据选中的 k 个点距离待预测点的距离不同，k 个点的权值也不同；

* P = n / k
* n：k 个样本中，最终确定的类型的个数；如下图，最终判断为 红色类型，概率：p = n/k = 2 / 3；

![image](https://user-images.githubusercontent.com/39177230/114677673-1e309b80-9d3d-11eb-824a-95d34a8abd55.png)

3. 决策树算法(descion tree)

* 通常在“叶子”节点处的信息熵或者基尼系数不为 0，数据集中包含多种类别的数据，以数量最多的样本对应的类别作为最终的预测结果；（和 kNN 算法类似）
* P = n / N 
* n：“叶子”中数量最多的样本的类型对应的样本数量；
* N：“叶子”中样本总量；


4 支持向量机SVM 算法
* 在 scikit-learn 中的 SVC() 中的一个参数：probability
* probability = True：SVC() 返回样本为各个类别的概率；（默认为 False）
* 
```python
from sklearn.svm import SVC
svc = SVC(probability=True)
```








Reference: [集成学习 (投票分类器) ](https://blog.csdn.net/soullines/article/details/103994749)





