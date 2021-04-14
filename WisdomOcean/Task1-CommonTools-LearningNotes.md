### 1. Notebook ###

运行结果： [Task1-GeoDataAnalsysisTool.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/WisdomOcean/Task1-GeoDataAnalsysisTool.ipynb)

### 2. Notebook Study ###

#### 2.1 Python Package: [Shapely](https://pypi.org/project/Shapely/) ####

Manipulation and analysis of geometric objects in the Cartesian plane.

Shapely is a BSD-licensed Python package for manipulation and analysis of planar geometric objects. It is based on the widely deployed GEOS (the engine of PostGIS) and JTS (from which GEOS is ported) libraries. Shapely is not concerned with data formats or coordinate systems, but can be readily integrated with packages that are. For more details, see:

[Shapely GitHub repository](https://github.com/Toblerity/Shapely)
[Shapely documentation and manual](https://shapely.readthedocs.io/en/latest/)

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






