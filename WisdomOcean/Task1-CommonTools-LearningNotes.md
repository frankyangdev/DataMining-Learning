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

#### 2.2 Python Package: [GeoPandas](https://geopandas.org/)####

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









### Reference:###

[GeoPandas，几行代码实现点转线功能](https://blog.csdn.net/u012413551/article/details/93535357)










