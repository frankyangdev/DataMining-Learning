### 1. Notebook ###

运行结果: [T2-EDA.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-EDA.ipynb)

[Pandas_Profiling Report](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-PandasProfilingReport.zip)




### 2. EDA Study ###

![image](https://user-images.githubusercontent.com/39177230/114896545-05aaa900-9e43-11eb-819d-00a1439f7944.png)


**Exploratory Data Analysis(EDA):** Exploratory data analysis is a complement to inferential statistics, which tends to be fairly rigid with rules and formulas. At an advanced level, EDA involves looking at and describing the data set from different angles and then summarizing it.

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



**Reference**：

[Python数据分析EDA](https://blog.csdn.net/weixin_33201531/article/details/112898635)

[Exploratory Data Analysis(EDA) From Scratch in Python](https://medium.com/swlh/exploratory-data-analysis-eda-from-scratch-in-python-8c12c2673aa7)

[](https://medium.com/analytics-vidhya/basic-statistics-in-data-science-38245e9b32bf)



