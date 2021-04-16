### 运行后 Notebook [Task2-EDA.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/WisdomOcean/Task2-EDA.ipynb)###


### 导入外部read_all_data.py ###

![image](https://user-images.githubusercontent.com/39177230/115046024-9a77da00-9f09-11eb-8bfe-cfc7dcb32636.png)

### 查看数据是否有重复 ###

![image](https://user-images.githubusercontent.com/39177230/115048902-af09a180-9f0c-11eb-8c8d-3c12b288e191.png)

### 查看数据集中特征缺失值、唯一值等

**nunique()**

```python
one_value_fea = [col for col in data_train.columns if data_train[col].nunique() <= 1]
```

### 随机取三个投网类型 可视化轨迹###

如果每个类型区别不明显，可以考虑其他图形

![image](https://user-images.githubusercontent.com/39177230/115049421-4969e500-9f0d-11eb-8dc7-85778a2b7967.png)


### 坐标序列可视化 ###

![image](https://user-images.githubusercontent.com/39177230/115049969-dca31a80-9f0d-11eb-8907-f8f124ec685e.png)


### 三类渔船速度和方向可视化 ###

![image](https://user-images.githubusercontent.com/39177230/115050114-00666080-9f0e-11eb-8659-1a9366ea869d.png)

### 三类渔船速度和方向的数据分布 ###

![image](https://user-images.githubusercontent.com/39177230/115050401-4c190a00-9f0e-11eb-87e7-690600b519cf.png)

### 分位图对速度和方向的数据分布进行可视化 ###

![image](https://user-images.githubusercontent.com/39177230/115050690-969a8680-9f0e-11eb-8af8-9ed2f0f4d5c3.png)


