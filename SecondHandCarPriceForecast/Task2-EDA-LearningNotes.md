### 1. Notebook ###

运行结果: [T2-EDA.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-EDA.ipynb)

[Pandas_Profiling Report](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-PandasProfilingReport.zip)




### 2. Code Study ###

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

**总结： 和pandas_profiling比较，Sweetviz 更sweet一些，提供比较详细的取值明细**

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

