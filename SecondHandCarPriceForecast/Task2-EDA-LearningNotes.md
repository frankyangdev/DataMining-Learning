### 1. Notebook ###

运行结果: [T2-EDA.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-EDA.ipynb)

[Pandas_Profiling Report](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-PandasProfilingReport.zip)




### 2. EDA Study ###

![image](https://user-images.githubusercontent.com/39177230/114896545-05aaa900-9e43-11eb-819d-00a1439f7944.png)


**Exploratory Data Analysis(EDA):** Exploratory data analysis is a complement to inferential statistics, which tends to be fairly rigid with rules and formulas. At an advanced level, EDA involves looking at and describing the data set from different angles and then summarizing it.

![image](https://user-images.githubusercontent.com/39177230/114896961-5f12d800-9e43-11eb-9c11-a95a3c0a043d.png)


* Descriptive Statistics:

The type of statistics dealing with numbers (numerical facts, figures, or information) to describe any phenomena. These numbers are descriptive statistics. e.g. Reports of industry production, cricket batting averages, government deficits, Movie Ratings etc.

* Inferential statistics

Inferential statistics is a decision, estimate, prediction, or generalization about a population, based on a sample. A population is a collection of all possible individual, objects, or measurements of interest. A sample is a portion, or part, of the population of interest. Inferential statistics is used to make inferences from data whereas descriptive statistics simply describe what’s going on in our data.


#### 2.1 Python Package [pandas-profiling](https://pypi.org/project/pandas-profiling/)


Generates profile reports from a pandas DataFrame.

The pandas df.describe() function is great but a little basic for serious exploratory data analysis. pandas_profiling extends the pandas DataFrame with df.profile_report() for quick data analysis.

For each column the following statistics - if relevant for the column type - are presented in an interactive HTML report:

* Type inference: detect the types of columns in a dataframe.
* Essentials: type, unique values, missing values
* Quantile statistics like minimum value, Q1, median, Q3, maximum, range, interquartile range
* Descriptive statistics like mean, mode, standard deviation, sum, median absolute deviation, coefficient of variation, kurtosis, skewness
* Most frequent values
* Histogram
* Correlations highlighting of highly correlated variables, Spearman, Pearson and Kendall matrices
* Missing values matrix, count, heatmap and dendrogram of missing values
* Text analysis learn about categories (Uppercase, Space), scripts (Latin, Cyrillic) and blocks (ASCII) of text data.
* File and Image analysis extract file sizes, creation dates and dimensions and scan for truncated images or those containing EXIF information.

```
!pip3 install pandas-profiling
```

```python
import pandas_profiling

pfr = pandas_profiling.ProfileReport(Train_data)
pfr.to_file("./example.html")

```

![image](https://user-images.githubusercontent.com/39177230/114893469-4ead2e00-9e40-11eb-8346-fd8c5de6f2ea.png)

![image](https://user-images.githubusercontent.com/39177230/114886571-39350580-9e3a-11eb-9e7a-df201770e073.png)

#### 2.2 Python Package [sweetviz](https://pypi.org/project/sweetviz/)

**总结：**

* 和pandas_profiling比较，Sweetviz 更sweet一些，提供比较详细的取值明细
* 生成的html文件远小于pandas_profling,可以根据实际需要选取其中一种

Sweetviz is an open-source Python library that generates beautiful, high-density visualizations to kickstart EDA (Exploratory Data Analysis) with just two lines of code. Output is a fully self-contained HTML application.

The system is built around quickly visualizing target values and comparing datasets. Its goal is to help quick analysis of target characteristics, training vs testing data, and other such data characterization tasks.

**Features **

* Target analysis

  * Shows how a target value (e.g. "Survived" in the Titanic dataset) relates to other features
  
* Visualize and compare

  * Distinct datasets (e.g. training vs test data)
  
  * Intra-set characteristics (e.g. male versus female)
  
* Mixed-type associations

  * Sweetviz integrates associations for numerical (Pearson's correlation), categorical (uncertainty coefficient) and categorical-numerical (correlation ratio) datatypes seamlessly, to provide maximum information for all data types.

* Type inference

  * Automatically detects numerical, categorical and text features, with optional manual overrides

* Summary information
  
  * Type, unique values, missing values, duplicate rows, most frequent values
  
  * Numerical analysis:
  
  * min/max/range, quartiles, mean, mode, standard deviation, sum, median absolute deviation, coefficient of variation, kurtosis, skew

```
!pip3 install sweetviz
```

```python
import sweetviz as sv

my_report = sv.analyze(Train_data)
my_report.show_html()
```

![image](https://user-images.githubusercontent.com/39177230/114892080-04777d00-9e3f-11eb-9be1-b55ac6615e98.png)


![image](https://user-images.githubusercontent.com/39177230/114891944-e873db80-9e3e-11eb-9473-34aacea3a7be.png)

#### 2.3 Python Pacakge: [PandasGUI](https://pypi.org/project/pandasgui/) ####

PandasGUI与前面的两个不同，PandasGUI不会生成报告，而是生成一个GUI(图形用户界面)的数据框，可以使用它来更详细地分析Dataframe。

```
!pip3 install pandasgui
```

```python
from pandasgui import show

gui = show(mpg)
```

在此GUI中，可以做很多事情，比如过滤、统计信息、在变量之间创建图表、以及重塑数据。这些操作可以根据需求拖动选项卡来完成。
绘图器功能,用它进行拖拽操作简直和excel没有啥区别

还可以通过创建新的数据透视表或者融合数据集来进行重塑。

然后，处理好的数据集可以直接导出成csv。

**总结**

* Pandas Profiling 适用于快速生成单个变量的分析。

* Sweetviz 适用于数据集之间和目标变量之间的分析。

* PandasGUI适用于具有手动拖放功能的深度分析。


#### 2.4 数据处理 ####

1. 处理缺失值 (Handling missing value) 

* Drop the missing values: In this case, we drop the missing values from those variables. In case there are very few missing values you can drop those values.
删除缺失值：在这种情况下，我们从那些变量中删除缺失值。 如果缺少的值很少，则可以删除这些值。

* Impute with mean value: For the numerical column, you can replace the missing values with mean values. Before replacing with mean value, it is advisable to check that the variable shouldn’t have extreme values .i.e. outliers.
用平均值估算：对于数字列，您可以用平均值替换缺失值。 在用平均值代替之前，建议检查变量不应该具有极高的值，即离群值。

* Impute with median value: For the numerical column, you can also replace the missing values with median values. In case you have extreme values such as outliers it is advisable to use the median approach.
用中值估算：对于数字列，您也可以用中值替换缺失值。 如果您有极端值(如离群值)，建议使用中位数法。

* Impute with mode value: For the categorical column, you can replace the missing values with mode values i.e the frequent ones.
带模式值的插补：对于类别列，您可以将缺失值替换为模式值，即频繁的值。

2. 处理重复记录 (Handling Duplicate records)

```python
df.duplciated()

df.drop_duplicates(inplace=True)

df1 = df.duplciated()
df1.sum()
```

3. 处理异常值 (Handling Outlier)

作为最极端观察值的异常值可能包括样本最大值或样本最小值，或两者都包括，这取决于它们是极高还是极低。 但是，样本的最大值和最小值并不总是离群值，因为它们可能与其他观测值相距不远。
通常，我们借助boxplot识别异常值，因此这里的box plot显示了数据范围之外的一些数据点。

* Drop the outlier value
降低离群值

* Replace the outlier value using the IQR
使用IQR替换离群值

4. 双变量分析 (Bivariate Analysis)

当我们谈论双变量分析时，它意味着分析2个变量。 由于我们知道存在数值变量和类别变量，因此有一种分析这些变量的方法，如下所示：

Numerical vs. Numerical1. Scatterplot2. Line plot3. Heatmap for correlation4. Joint plot

Categorical vs. Numerical1. Bar chart2. Voilin plot3. Categorical box plot4.Swarm plot

Two Categorcal Variables1. Bar chart2. Grouped bar chart3. Point plot

5. 规范化和缩放 (Normalizing and Scaling)

特征缩放(也称为数据规范化)是用于标准化数据特征范围的方法。 由于数据值的范围可能相差很大，因此它成为使用机器学习算法时数据预处理的必要步骤。
在这种方法中，我们将具有不同度量标准的变量转换为单个尺度。 StandardScaler使用公式(x-均值)/标准差对数据进行归一化。 我们将仅对数字变量执行此操作。

6. 编码 (ENCODING)

One-Hot-Encoding is used to create dummy variables to replace the categories in a categorical variable into features of each category and represent it using 1 or 0 based on the presence or absence of the categorical value in the record.

一键编码用于创建伪变量，以将分类变量中的类别替换为每个类别的特征，并根据记录中是否存在分类值使用1或0表示它。

This is required to do since the machine learning algorithms only work on the numerical data. That is why there is a need to convert the categorical column into a numerical one.

由于机器学习算法仅对数值数据起作用，因此需要这样做。 这就是为什么需要将分类列转换为数字列的原因。


**Reference**：

[Python数据分析EDA](https://blog.csdn.net/weixin_33201531/article/details/112898635)

[Exploratory Data Analysis(EDA) From Scratch in Python](https://medium.com/swlh/exploratory-data-analysis-eda-from-scratch-in-python-8c12c2673aa7)

[Basic statistics in Data science](https://medium.com/analytics-vidhya/basic-statistics-in-data-science-38245e9b32bf)

[Python 探索性数据分析 EDA](https://blog.csdn.net/weixin_26705651/article/details/108497769)



