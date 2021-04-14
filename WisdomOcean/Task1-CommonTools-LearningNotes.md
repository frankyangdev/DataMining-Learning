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

**Usage Sample**

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





