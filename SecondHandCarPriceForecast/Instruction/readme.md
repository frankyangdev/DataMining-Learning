﻿

## 简介

[数据挖掘实践（二手车价格预测）](https://github.com/datawhalechina/team-learning-data-mining/tree/master/SecondHandCarPriceForecast)的内容来自 Datawhale与天池联合发起的 [零基础入门数据挖掘 - 二手车交易价格预测](https://tianchi.aliyun.com/competition/entrance/231784/introduction) 学习赛的第一场。

![](https://img-blog.csdnimg.cn/20200801001301791.png)

- [零基础入门数据挖掘 - 二手车交易价格预测](https://tianchi.aliyun.com/competition/entrance/231784/introduction)
- [Baseline](https://tianchi.aliyun.com/notebook-ai/detail?postId=95422)
- [Task1 赛题理解](https://tianchi.aliyun.com/notebook-ai/detail?postId=95456)
- [Task2 数据分析](https://tianchi.aliyun.com/notebook-ai/detail?postId=95457)
- [Task3 特征工程](https://tianchi.aliyun.com/notebook-ai/detail?postId=95501)
- [Task4 建模调参](https://tianchi.aliyun.com/notebook-ai/detail?postId=95460)
- [Task5 模型融合](https://tianchi.aliyun.com/notebook-ai/detail?postId=95535)
- [录播01：Baseline + 赛题理解](https://tianchi.aliyun.com/course/video?spm=5176.12282064.0.0.25382042MPEAvp&liveId=41143)
- [录播02：特征工程](https://tianchi.aliyun.com/course/video?spm=5176.12282064.0.0.25382042MPEAvp&liveId=41145)
- [录播03：建模和调参](https://tianchi.aliyun.com/course/live?liveId=41146)
- [录播04：模型融合](https://tianchi.aliyun.com/course/video?liveId=41149)


---
## 基本信息
- 贡献人员：王贺、王茂霖、薛传雨、车弘书、陈泽、谢文昕、鲁力、李文乐、姚鑫、苏鹏、六一、李玲、罗如意、徐何军、詹好
- 学习周期：14天，每天平均花费时间3小时-5小时不等，根据个人学习接受能力强弱有所浮动。
- 学习形式：理论学习 + 练习
- 人群定位：有一定数据分析与python编程的基础。
- 先修内容：[Python编程语言](https://github.com/datawhalechina/team-learning-program/tree/master/Python-Language)
- 难度系数：中


---
## 任务安排

### Task01：赛题理解

<b>学习目标</b>

- 理解赛题数据和目标，清楚评分体系。
- 完成相应报名，下载数据和结果提交打卡（可提交示例结果），熟悉比赛流程

<b>了解赛题</b>

- 赛题概况
- 数据概况
- 预测指标
- 分析赛题

<b>代码示例</b>

- 数据读取pandas
- 分类指标评价计算示例
- 回归指标评价计算示例


### Task02：数据分析



<b>EDA目标</b>

- EDA的价值主要在于熟悉数据集，了解数据集，对数据集进行验证来确定所获得数据集可以用于接下来的机器学习或者深度学习使用。
- 当了解了数据集之后我们下一步就是要去了解变量间的相互关系以及变量与预测值之间的存在关系。
- 引导数据科学从业者进行数据处理以及特征工程的步骤,使数据集的结构和特征集让接下来的预测问题更加可靠。
- 完成对于数据的探索性分析，并对于数据进行一些图表或者文字总结并打卡。

**代码示例**

- 载入各种数据科学以及可视化库
- 载入数据
- 总览数据概况
- 判断数据缺失和异常
- 了解预测值的分布
- 特征分为类别特征和数字特征，并对类别特征查看unique分布
- 数字特征分析
- 类别特征分析
- 用pandas_profiling生成数据报告



### Task03：特征工程



<b>特征工程目标</b>

 
- 对于特征进行进一步分析，并对于数据进行处理。
- 完成对于特征工程的分析，并对于数据进行一些图表或者文字总结并打卡。


<b>代码示例</b>
- 导入数据
- 删除异常值
- 特征构造
- 特征筛选



### Task04：建模调参



<b>学习目标</b>

- 了解常用的机器学习模型，并掌握机器学习模型的建模与调参流程
- 完成相应学习打卡任务


<b>相关原理介绍与推荐</b>
- 线性回归模型
- 决策树模型
- GBDT模型
- XGBoost模型
- LightGBM模型
- 推荐教材

<b>代码示例</b>
- 读取数据
- 线性回归 & 五折交叉验证 & 模拟真实业务情况
- 多种模型对比
- 模型调参



### Task05：模型融合



<b>模型融合目标</b>

 
- 对于多种调参完成的模型进行模型融合。
- 完成对于多种模型的融合，提交融合结果并打卡。


<b>代码示例</b>
- 回归\分类概率-融合
- 分类模型融合
- 一些其它方法
- 本赛题示例






