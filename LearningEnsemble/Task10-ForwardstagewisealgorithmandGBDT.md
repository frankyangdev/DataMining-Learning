### 前向分步算法 Forward Stagewise algorithm ###

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
  
### GBDT 的全称是 Gradient Boosting Decision Tree，梯度提升树 ###

![image](https://user-images.githubusercontent.com/39177230/115911458-b3036980-a4a0-11eb-83ab-b8a6f11a03d1.png)









原文链接：https://blog.csdn.net/FeynmanWang/article/details/47100867
