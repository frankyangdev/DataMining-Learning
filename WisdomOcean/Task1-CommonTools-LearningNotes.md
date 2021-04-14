### 1. Notebook ###

运行结果： [Task1-GeoDataAnalsysisTool.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/WisdomOcean/Task1-GeoDataAnalsysisTool.ipynb)

### 2. Notebook Study ###

#### 2.1 Python Package: [Shapely](https://pypi.org/project/Shapely/) ####

Manipulation and analysis of geometric objects in the Cartesian plane.

Shapely is a BSD-licensed Python package for manipulation and analysis of planar geometric objects. It is based on the widely deployed GEOS (the engine of PostGIS) and JTS (from which GEOS is ported) libraries. Shapely is not concerned with data formats or coordinate systems, but can be readily integrated with packages that are. For more details, see:

[Shapely GitHub repository](https://github.com/Toblerity/Shapely)

[Shapely documentation and manual](https://shapely.readthedocs.io/en/latest/)

[user manual](https://shapely.readthedocs.io/en/latest/manual.html#introduction)

**Installation**
Linux, OS X, and Windows users can get Shapely wheels with GEOS included from the Python Package Index with a recent version of pip (8+):

```
$ pip install shapely
```

**Deployment & Testing**

```
$ virtualenv .
$ source bin/activate
(env)$ pip install -r requirements-dev.txt
(env)$ pip install -e .
```
```
(env)$ python -m pytest
```
**Integration**

Shapely does not read or write data files, but it can serialize and deserialize using several well known formats and protocols. The shapely.wkb and shapely.wkt modules provide dumpers and loaders inspired by Python's pickle module.

```python
>>> from shapely.wkt import dumps, loads
>>> dumps(loads('POINT (0 0)'))
'POINT (0.0000000000000000 0.0000000000000000)'
```

Shapely can also integrate with other Python GIS packages using GeoJSON-like dicts.

```python
>>> import json
>>> from shapely.geometry import mapping, shape
>>> s = shape(json.loads('{"type": "Point", "coordinates": [0.0, 0.0]}'))
>>> s
<shapely.geometry.point.Point object at 0x...>
>>> print(json.dumps(mapping(s)))
{"type": "Point", "coordinates": [0.0, 0.0]}
```

**Usage Example**

Here is the canonical example of building an approximately circular patch by buffering a point.

```python
>>> from shapely.geometry import Point
>>> patch = Point(0.0, 0.0).buffer(10.0)
>>> patch
<shapely.geometry.polygon.Polygon object at 0x...>
>>> patch.area
>>> 
313.65484905459385
```

#### 2.2 Python Package: [GeoPandas](https://geopandas.org/) ####

[geopandas in GitHub](https://github.com/geopandas/geopandas)

GeoPandas is an open source project to make working with geospatial data in python easier. GeoPandas extends the datatypes used by pandas to allow spatial operations on geometric types. Geometric operations are performed by shapely. Geopandas further depends on fiona for file access and matplotlib for plotting.

GeoPandas enables you to easily do operations in python that would otherwise require a spatial database such as PostGIS.

** Installation **

GeoPandas depends on the following packages:

* pandas
* shapely
* fiona
* pyproj

```
pip install geopandas
```
**Usage Example**

```python
>>> import geopandas
>>> from shapely.geometry import Polygon
>>> p1 = Polygon([(0, 0), (1, 0), (1, 1)])
>>> p2 = Polygon([(0, 0), (1, 0), (1, 1), (0, 1)])
>>> p3 = Polygon([(2, 0), (3, 0), (3, 1), (2, 1)])
>>> g = geopandas.GeoSeries([p1, p2, p3])
>>> g
0         POLYGON ((0 0, 1 0, 1 1, 0 0))
1    POLYGON ((0 0, 1 0, 1 1, 0 1, 0 0))
2    POLYGON ((2 0, 3 0, 3 1, 2 1, 2 0))
dtype: geometry
```
![image](https://user-images.githubusercontent.com/39177230/114685504-83d45600-9d44-11eb-9c38-9c9df196536e.png)

**Load Data**

```python
import pandas as pd
import geopandas as gpd
from shapely.geometry import LineString,Point

fp = r'./data/geo.csv'
df = pd.read_csv(fp)
df.iloc[:30,:]
```
![image](https://user-images.githubusercontent.com/39177230/114686484-68b61600-9d45-11eb-831f-3d1d92729905.png)


**print point**

```python
xy = [Point(xy) for xy in zip(df.Lng,df.Lat)]
pointDataFrame = gpd.GeoDataFrame(df,geometry=xy)
pointDataFrame.plot(figsize = (24, 24))
```

![image](https://user-images.githubusercontent.com/39177230/114686718-a2871c80-9d45-11eb-8c69-0666e9917956.png)

**点转线**
线是由点构成的，其在物理结构上表现的就是一连串有序排列的点。根据这一特征，我们来进行点转线操作。

将点转换为线：
1. num值相同的点，合并成一根线段；
2. 线段上的点的排列顺序，按照其在表中的排列顺序FID

```python
#分组
dataGroup = df.groupby('Num')

#构造数据
tb = []
geomList = []
for name,group in dataGroup:
    # 分离出属性信息，取每组的第1行前5列作为数据属性
    tb.append(group.iloc[0,:5])
    # 把同一组的点打包到一个list中
    xyList = [xy for xy in zip(group.Lng, group.Lat)]
    
    line = LineString(xyList)
    geomList.append(line)

# 点转线
geoDataFrame = gpd.GeoDataFrame(tb, geometry = geomList)

```

```python
geoDataFrame.iloc[:20,:]
```
![image](https://user-images.githubusercontent.com/39177230/114687060-f265e380-9d45-11eb-82d3-62789d3b2832.png)

数据已经按Num分组，并合并成了一个LineString对象。

```python
##通过地图显示查看结果。
geoDataFrame.plot(figsize = (24, 24))

```

![image](https://user-images.githubusercontent.com/39177230/114687197-16c1c000-9d46-11eb-8168-a68c50f56d90.png)

**将结果保存为geojson文件**
1. 保存为geojson

```python
# 方法一 
fp = r"E:\Dev\data\lineRoads.geojson"
geoDataFrame.to_file(fp, driver='GeoJSON', encoding="utf-8")

#方法二
json = geoDataFrame.to_json()
with open(fp,'w') as f:
    f.write(json)
```

3. 保存为shp

```python
shp = r"E:\Dev\data\lineRoads.shp"
geoDataFrame.to_file(shp,driver="ESRI Shapefile",encoding="utf-8")
```

#### 2.3 Python Package: [keplergl](https://pypi.org/project/keplergl/) ####

[keplergl in GitHub](https://github.com/keplergl/kepler.gl) 

[KeplerGL-jupyter](https://docs.kepler.gl/docs/keplergl-jupyter) 



Kepler.gl is a data-agnostic, high-performance web-based application for visual exploration of large-scale geolocation data sets. Built on top of Mapbox GL and deck.gl, kepler.gl can render millions of points representing thousands of trips and perform spatial aggregations on the fly.

Kepler.gl is also a React component that uses Redux to manage its state and data flow. It can be embedded into other React-Redux applications and is highly customizable. 

**Installation**

```python
$ pip install keplergl
```

**Usage Example**

1. Load keplergl map

```
KeplerGl()
```


```python
# Load an empty map
from keplergl import KeplerGl
map_1 = KeplerGl()
map_1
```

```python
# Load a map with data and config and height
from keplergl import KeplerGl
map_2 = KeplerGl(height=400, data={"data_1": my_df}, config=config)
map_2
```

2. Add Data

```
.add_data()
```

kepler.gl expected the data to be CSV, GeoJSON, DataFrame or GeoDataFrame. You can call add_data multiple times to add multiple datasets to kepler.gl

```python
# DataFrame
df = pd.read_csv('hex-data.csv')
map_1.add_data(data=df, name='data_1')

# CSV
with open('csv-data.csv', 'r') as f:
    csvData = f.read()
map_1.add_data(data=csvData, name='data_2')

# GeoJSON as string
with open('sf_zip_geo.json', 'r') as f:
    geojson = f.read()

map_1.add_data(data=geojson, name='geojson')
```

3. Customize the map
Interact with kepler.gl and customize layers and filters. Map data and config will be stored locally to the widget state. To make sure the map state is saved, select Widgets > Save Notebook Widget State, before shutting down the kernel.


4. Save and load config
```
.config
```

```python
map_1.config
## {u'config': {u'mapState': {u'bearing': 2.6192893401015205,
#  u'dragRotate': True,
#   u'isSplit': False,
#   u'latitude': 37.76209132041332,
#   u'longitude': -122.42590232651203,
```

5.  Match config with data

All layers, filters and tooltips are associated with a specific dataset. Therefore the data and config in the map has to be able to match each other. The name of the dataset is assigned to:

* dataId of layer.config,
* dataId of filter
* key in interactionConfig.tooltip.fieldToShow.

![image](https://user-images.githubusercontent.com/39177230/114693130-df561200-9d4b-11eb-9b9d-237afbdb1d83.png)


you can print your current map configuration at any time in the notebook

6. Save Map

When you click in the map and change settings, config is saved to widget state. Closing the notebook and reopen it will reload current map. However, you need to manually select Widget > Save Notebook Widget State before shut downing the kernel to make sure it will be reloaded.

```
.save_to_html()
```
You can export your current map as an interactive html file

```python
# this will save current map
map_1.save_to_html(file_name='first_map.html')

# this will save map with provided data and config
map_1.save_to_html(data={'data_1': df}, config=config, file_name='first_map.html')

# this will save map with the interaction panel disabled
map_1.save_to_html(file_name='first_map.html', read_only=True)
```

```
._repr_html_()
```
You can also directly serve the current map via a flask app. To do that return kepler’s map HTML representation.

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return map_1._repr_html_()

if __name__ == '__main__':
    app.run(debug=True)

```

**What's your python and node env?**

* Python:

```
python==3.7.4
notebook==6.0.3
jupyterlab==2.1.2
ipywidgets==7.5.1
```

* Node (Only for JupyterLab)
```
node==8.15.0
yarn==1.7.0
```




#### 2.4 道格拉斯-普克算法(Douglas–Peucker algorithm) ####

道格拉斯-普克算法(Douglas–Peucker algorithm，亦称为拉默-道格拉斯-普克算法、迭代适应点算法、分裂与合并算法)是将曲线近似表示为一系列点，并减少点的数量的一种算法。该算法的原始类型分别由乌尔斯·拉默（Urs Ramer）于1972年以及大卫·道格拉斯（David Douglas）和托马斯·普克（Thomas Peucker）于1973年提出，并在之后的数十年中由其他学者予以完善。

[Ramer-Douglas-Peucker Algorithm](https://rdp.readthedocs.io/en/latest/)

经典的Douglas-Peucker算法描述如下：

* 在曲线首尾两点A，B之间连接一条直线AB，该直线为曲线的弦；

* 得到曲线上离该直线段距离最大的点C，计算其与AB的距离d；

* 比较该距离与预先给定的阈值threshold的大小，如果小于threshold，则该直线段作为曲线的近似，该段曲线处理完毕。

* 如果距离大于阈值，则用C将曲线分为两段AC和BC，并分别对两段取信进行1~3的处理。

* 当所有曲线都处理完毕时，依次连接各个分割点形成的折线，即可以作为曲线的近似。


The Ramer–Douglas–Peucker algorithm (RDP) is an algorithm for reducing the number of points in a curve that is approximated by a series of points.


<img src="https://rdp.readthedocs.io/en/latest/_images/rdp.gif" alt="Ramer-Douglas-Peucker Algorithm" width="472"/>


#### 2.5 Python Package [geohash](https://pypi.org/project/python-geohash/)

**[geohash in GitHub](https://github.com/vinsci/geohash) **

* GeoHash是一种地址编码方法。他能够把二维的空间经纬度数据编码成一个字符串。GeoHash具有以下特点：
1. GeoHash用一个字符串表示经度和纬度两个坐标。在数据库中可以实现在一列上应用索引
2. GeoHash表示的并不是一个点，而是一个区域；
3. GeoHash编码的前缀可以表示更大的区域。例如wx4g0ec1，它的前缀wx4g0e表示包含编码wx4g0ec1在内的更大范围。 这个特性可以用于附近地点搜索
* 计算方法：
GeoHash的计算过程分为三步：
1. 将经纬度转换成二进制：
比如这样一个点（39.923201, 116.390705）
纬度的范围是（-90，90），其中间值为0。对于纬度39.923201，在区间（0，90）中，因此得到一个1；（0，90）区间的中间值为45度，纬度39.923201小于45，因此得到一个0，依次计算下去，即可得到纬度的二进制表示，如下表：

![image](https://user-images.githubusercontent.com/39177230/114714371-a4140d00-9d64-11eb-8bbd-03d71901a0f1.png)


最后得到纬度的二进制表示为：
10111000110001111001
同理可以得到经度116.390705的二进制表示为：
11010010110001000100

2. 合并纬度、经度的二进制：
合并方法是将经度、纬度二进制按照奇偶位合并：
11100 11101 00100 01111 00000 01101 01011 00001
3. 按照Base32进行编码：
Base32编码表（其中一种）：

![image](https://user-images.githubusercontent.com/39177230/114714548-cc037080-9d64-11eb-9359-6eb8ccabcfa6.png)

将上述合并后二进制编码后结果为：wx4g0ec1

3. GeoHash的精度

编码越长，表示的范围越小，位置也越精确。因此我们就可以通过比较GeoHash匹配的位数来判断两个点之间的大概距离。

![image](https://user-images.githubusercontent.com/39177230/114714731-f6edc480-9d64-11eb-8171-7945a84fe396.png)

4. 不足之处及解决方法
* 边缘附近的点，黄色的点要比黑色的点更加靠近红点，但是由于黑点跟红点的GeoHash前缀匹配数目更多，因此得到黑点更加靠近红点的结果

解决方法：
可以通过筛选周围8个区域内的所有点，然后计算距离得到满足条件结果。
* 因为现有的GeoHash算法使用的是Peano空间填充曲线（可感兴趣的可自己查看），这种曲线会产生突变，造成了编码虽然相似但距离可能相差很大的问题，因此在查询附近的时候，首先筛选GeoHash编码相似的点，然后进行实际距离计算。

![image](https://user-images.githubusercontent.com/39177230/114714960-2c92ad80-9d65-11eb-90af-20331897512f.png)



#### 2.6 Python Package Pickle ####

[pickle — Python object serialization](https://docs.python.org/3/library/pickle.html)

The pickle module implements binary protocols for serializing and de-serializing a Python object structure. “Pickling” is the process whereby a Python object hierarchy is converted into a byte stream, and “unpickling” is the inverse operation, whereby a byte stream (from a binary file or bytes-like object) is converted back into an object hierarchy. Pickling (and unpickling) is alternatively known as “serialization”, “marshalling,” 1 or “flattening”; however, to avoid confusion, the terms used here are “pickling” and “unpickling”.

```
**Warning**  The pickle module is not secure. Only unpickle data you trust.
It is possible to construct malicious pickle data which will execute arbitrary code during unpickling. Never unpickle data that could have come from an untrusted source, or that could have been tampered with.

Consider signing data with hmac if you need to ensure that it has not been tampered with.

Safer serialization formats such as json may be more appropriate if you are processing untrusted data. See Comparison with json.
```

pickle提供了一个简单的持久化功能。可以将对象以文件的形式存放在磁盘上。

pickle模块只能在python中使用，python中几乎所有的数据类型（列表，字典，集合，类等）都可以用pickle来序列化，

pickle序列化后的数据，可读性差，人一般无法识别。


* 序列化对象，并将结果数据流写入到文件对象中。参数protocol是序列化模式，默认值为0，表示以文本的形式序列化。protocol的值还可以是1或2，表示以二进制的形式序列化

```
pickle.dump(obj, file[, protocol])
```

* 反序列化对象。将文件中的数据解析为一个Python对象。

```
pickle.load(file)
```





### Reference: ###

[GeoPandas，几行代码实现点转线功能](https://blog.csdn.net/u012413551/article/details/93535357)

[道格拉斯-普克算法(Douglas–Peucker algorithm)](https://blog.csdn.net/deram_boy/article/details/39177015)

[Python pickle模块学习](https://blog.csdn.net/chunmi6974/article/details/78392230)

[地理空间索引：GeoHash原理](https://blog.csdn.net/zhufenghao/article/details/85568340)

[GeoHash简介](https://blog.csdn.net/youhongaa/article/details/78816700)











