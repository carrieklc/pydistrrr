# Pydistrrr

[![Build Status](https://travis-ci.org/UBC-MDS/pydistrrr.svg?branch=master)](https://travis-ci.org/UBC-MDS/pydistrrr)

|Contributors|GitHub Handle|
|------------|-------------|
|Carrie Cheung| [carrieklc](https://github.com/carrieklc)|
|Evan Yathon|[EvanYathon](https://github.com/EvanYathon)|
|Mike Yuan|[mikeymice](https://github.com/mikeymice)|
|Shayne Andrews|[shayne-andrews](https://github.com/shayne-andrews)|

## Project Summary
`pydistrrr` is a Python package that calculates distances between numeric-based data points or observations. The currently supported distance metrics are:

* [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance)
* [Manhattan Distance](https://en.wikipedia.org/wiki/Taxicab_geometry)
* [Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity)

In addition to computing distances, `pydistrrr` can identify the closest data points to a given point based on a distance threshold, or based on a user-specified number of points.  These functions are designed to be similar to [Scikit Learn's Nearest Neighbors](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors.kneighbors) functionality.

## Test Coverage
We used the Python framework [`pytest`](https://docs.pytest.org/en/latest/) and the plug-in [`pytest-cov`](https://pypi.org/project/pytest-cov/) to test and track test coverage for the `pydistrrr` package. The results of the coverage report can be seen below.

![](reports/pydistrrr_test_coverage2.png)

## Functions

| Function Name | Input | Output | Description |
|-------------|-----|------|-----------|
|get_distance|3 parameters:  2 lists of numeric values, a string specifying type of distance metric | Single float| Given 2 observations each represented by a list of numeric values, compute and return the distance between the 2 points based on the specified distance metric (e.g. `metric="euclidean"`)|
|get_all_distances |3 parameters:  a list of numeric values, a dataframe, a string specifying type of distance metric  | List of floats of length `n`| Given a dataframe and an observation represented by a list of numeric values, compute and return the distances between the single observation and each observation in the dataframe based on the specified distance metric. Will output a list of distances (as numeric values) with size equal to the number of rows in the dataframe, `n`.|
|filter_distances| 4 parameters: a list of numeric values, a dataframe, a numeric (float or int) value representing a threshold distance, a string specifying type of distance metric |List of int (row indices)| Similiar to `get_all_distances` except indices of rows/observations with distances less than the threshold distance will be returned.|
|get_closest|4 parameters: a list specifying values for a target point, a dataframe of data points, an int for number of neighbours k, a string specifying type of distance metric to calculate |List of int (row indices) of length `k`| Similiar to `get_all_distances` except indices of the top `k` rows/observations with the smallest distances are returned. In the case where there is a tie in distances between two or more points, the point with larger index in the dataframe will be selected.|


## Alignment with Python / R Ecosystems

There are existing packages that implement the same proposed functionality in both Python and R (listed below). Most of these packages provide functions to calculate different distance metrics between observations and/or also extend the functionality to compute the k closest neighbours (KNN) of a given point based on a selected distance metric.

In our package, we will be implementing the distance metric calculations manually rather than simply creating wrappers around existing functions.


|Existing Packages/Functions|
|---------------------------|
| [Sklearn's NearestNeighbors](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors.kneighbors) |
| [Scipy's Spatial Distance Functions](https://docs.scipy.org/doc/scipy/reference/spatial.distance.html) |
| [R Distance Computations](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/dist.html) |
| [R K Nearest Neighbours](https://cran.r-project.org/web/packages/FNN/index.html) |

## Installation
To install the package, simply run the below in your terminal:

 `pip install git+https://github.com/UBC-MDS/pydistrrr.git`

Then simply import `pydistrrr` in your own development. For example:
```
>>> from pydistrrr import *
>>> get_distance([1,2],[2,1])
1.4142135623730951
```

## Example Usages
|Function Name|Example Usage(s)|
|--------|-------|
|get_distance|<code>get_distance([1,2], [2,1], "manhattan")</code>|
|get_all_distances|<code>x = [-2,4]<br>df = pd.DataFrame({"A" : [1,2,3], "B" : [8,2,4]})<br>get_all_distances(x, df, "cosine")</code>|
|filter_distances|<code>x = [1, 1]<br>df = pd.DataFrame([[1, 1], [1, 2], [1, 3]])<br>filter_distances(x, df, threshold=0.9, dist="euclidean")</code>|
|get_closest|<code>x = [1, 1] <br> df = pd.DataFrame([[1, 1], [1, 2], [1, 3]]) <br> get_closest(x, df, top_k=2, "manhattan")</code>|
