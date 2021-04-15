### 1. Notebook ###

运行结果: [T2-EDA.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-EDA.ipynb)

[Pandas_Profiling Report](https://github.com/frankyangdev/DataMining-Learning/blob/main/SecondHandCarPriceForecast/T2-PandasProfilingReport.zip)

![image](https://user-images.githubusercontent.com/39177230/114886571-39350580-9e3a-11eb-9e7a-df201770e073.png)


### 2. Code Study###

#### 2.1 Python Package [sweetviz](https://pypi.org/project/sweetviz/)

Sweetviz is an open-source Python library that generates beautiful, high-density visualizations to kickstart EDA (Exploratory Data Analysis) with just two lines of code. Output is a fully self-contained HTML application.

The system is built around quickly visualizing target values and comparing datasets. Its goal is to help quick analysis of target characteristics, training vs testing data, and other such data characterization tasks.

** Features **

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
