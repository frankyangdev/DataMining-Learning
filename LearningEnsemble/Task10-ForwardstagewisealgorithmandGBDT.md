### 1. 前向分步算法 Forward Stagewise algorithm ###

输入：训练数据集 T={(x1,y1),(x2,y2),⋯,(xN,yN)}T={(x1,y1),(x2,y2),⋯,(xN,yN)}；损失函数 L(y,f(x))L(y,f(x)) ；基函数集 {b(x;γ)}{b(x;γ)}；
 输出：加法模型 f(x)f(x) .
 (1) 初始化 f0(x)=0f0(x)=0
 (2) 对 m=1,2,⋯,Mm=1,2,⋯,M
 (a) 极小化损失函数
 
![image](https://user-images.githubusercontent.com/39177230/115910941-088b4680-a4a0-11eb-8e23-34ec3733ad59.png)
 
 
 得到参数 βm,γm
 (b) 更新
 
 ![image](https://user-images.githubusercontent.com/39177230/115910969-12ad4500-a4a0-11eb-8cf5-cc1c0f32d331.png)

 
  (3) 得到加法模型
  
  ![image](https://user-images.githubusercontent.com/39177230/115911014-235dbb00-a4a0-11eb-89c8-c33243c76ee3.png)

小明有100个苹果，小红第一次猜1*50个，剩余50个没猜对（残差），下一次小红猜有1*50 + 2*10个，残差30，如此反复下去，小红会逐渐向正确答案靠拢。
其中第一二次中的1*50和2*10中的1,2就相当于系数\beta_m，50和10可以理解为\gamma _{m}  
  
### 2. GBDT 的全称是 Gradient Boosting Decision Tree，梯度提升树 ###

![image](https://user-images.githubusercontent.com/39177230/115911458-b3036980-a4a0-11eb-83ab-b8a6f11a03d1.png)


### 3. [sklearn.ensemble.GradientBoostingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html#sklearn.ensemble.GradientBoostingRegressor) ###

GB builds an additive model in a forward stage-wise fashion; it allows for the optimization of arbitrary differentiable loss functions. In each stage a regression tree is fit on the negative gradient of the given loss function.

`class sklearn.ensemble.GradientBoostingRegressor(*, loss='ls', learning_rate=0.1, n_estimators=100, subsample=1.0, criterion='friedman_mse', min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_depth=3, min_impurity_decrease=0.0, min_impurity_split=None, init=None, random_state=None, max_features=None, alpha=0.9, verbose=0, max_leaf_nodes=None, warm_start=False, validation_fraction=0.1, n_iter_no_change=None, tol=0.0001, ccp_alpha=0.0)
`

```python
>>> from sklearn.datasets import make_regression
>>> from sklearn.ensemble import GradientBoostingRegressor
>>> from sklearn.model_selection import train_test_split
>>> X, y = make_regression(random_state=0)
>>> X_train, X_test, y_train, y_test = train_test_split(
...     X, y, random_state=0)
>>> reg = GradientBoostingRegressor(random_state=0)
>>> reg.fit(X_train, y_train)
GradientBoostingRegressor(random_state=0)
>>> reg.predict(X_test[1:2])
array([-61...])
>>> reg.score(X_test, y_test)
0.4...
```

### 4. [sklearn.ensemble.GradientBoostingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html?highlight=gra#sklearn.ensemble.GradientBoostingClassifier) ###

GB builds an additive model in a forward stage-wise fashion; it allows for the optimization of arbitrary differentiable loss functions. In each stage n_classes_ regression trees are fit on the negative gradient of the binomial or multinomial deviance loss function. Binary classification is a special case where only a single regression tree is induced.

`class sklearn.ensemble.GradientBoostingClassifier(*, loss='deviance', learning_rate=0.1, n_estimators=100, subsample=1.0, criterion='friedman_mse', min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_depth=3, min_impurity_decrease=0.0, min_impurity_split=None, init=None, random_state=None, max_features=None, verbose=0, max_leaf_nodes=None, warm_start=False, validation_fraction=0.1, n_iter_no_change=None, tol=0.0001, ccp_alpha=0.0)
`

The following example shows how to fit a gradient boosting classifier with 100 decision stumps as weak learners.

```python

>>>
>>> from sklearn.datasets import make_hastie_10_2
>>> from sklearn.ensemble import GradientBoostingClassifier
>>>
>>> X, y = make_hastie_10_2(random_state=0)
>>> X_train, X_test = X[:2000], X[2000:]
>>> y_train, y_test = y[:2000], y[2000:]
>>>
>>> clf = GradientBoostingClassifier(n_estimators=100, learning_rate=1.0,
...     max_depth=1, random_state=0).fit(X_train, y_train)
>>> clf.score(X_test, y_test)
0.913...
```









### Reference ###
1. [前向分布算法与GBDT算法梳理](https://blog.csdn.net/weixin_39982211/article/details/89048783)
2. [GBDT算法原理以及实例理解](https://blog.csdn.net/zpalyq110/article/details/79527653)
