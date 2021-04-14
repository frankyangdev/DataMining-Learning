
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

* hard: 独立个体的大多数类别就是改预测器的预测结果
```
模型1: A-0.9 B-0.1
模型2: A-0.8 B-0.9      # A 1票(模型1) B 3票   Hard voting预测结果B
模型3: A-0.3 B-0.2
模型4: A-0.4 B-0.3
```


* soft: 将每个预测器对某一类别的概率进行平均,最后横向比较

```
模型1: A-0.9 B-0.1
模型2: A-0.8 B-0.9      # A 平均概率:0.6 B 平均概率:0.375   Soft voting预测结果A
模型3: A-0.3 B-0.2
模型4: A-0.4 B-0.3
```



Reference: [集成学习 (投票分类器) ](https://blog.csdn.net/soullines/article/details/103994749)





