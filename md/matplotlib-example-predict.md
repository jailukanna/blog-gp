---
title: matplotlib example predict (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'predict'

Functions in program: 
* `def main():`
* `def predict(file, model):`
* `def get_model_quality(model):`
* `def visualize():`
* `def doDecisionTreeRegressor():`
* `def DecisionTree():`
* `def RandomForest():`
* `def KNF():`

Modules used in program: 
* `import gpxpy.gpx`
* `import gpxpy`
* `import os, sys`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import numpy as np`
* `import pandas as pd`
* `import pydotplus`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib`

## python predict

Python matplotlib example: predict

```python
#!/usr/bin/env python3

import matplotlib
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
from sklearn import tree

from sklearn.tree import DecisionTreeRegressor

from sklearn.datasets import load_iris
from sklearn.externals.six import StringIO 
from sklearn import tree
import pydotplus

import pandas as pd
import numpy as np
import seaborn as sns
from matplotlib import pyplot
from sklearn import neighbors, model_selection
from sklearn.model_selection import train_test_split
from pandas import plotting
import matplotlib.pyplot as plt
from sklearn import neighbors, model_selection, tree, ensemble
import seaborn as sns
from sklearn import linear_model
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.linear_model import Ridge
from sklearn.svm import SVC
import os, sys
from sys import stderr
import gpxpy
import gpxpy.gpx
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier


df = pd.read_csv('data.csv', header=None)

# Pandas dataframe to numpy.ndarray
X = df[[2,3]].values
y = df[1].values

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

def KNF():
    clf = KNeighborsClassifier(n_neighbors=3)
    clf.fit(X_train, y_train)
    return clf

def RandomForest():

    forest = RandomForestClassifier(n_estimators=10, random_state=5)
    forest = forest.fit(X_train, y_train)

    return forest

def DecisionTree():
    clf = tree.DecisionTreeClassifier()
    clf = clf.fit(X_train, y_train)

    dot_data = StringIO() 
    tree.export_graphviz(clf, out_file=dot_data) 
    graph = pydotplus.graph_from_dot_data(dot_data.getvalue()) 
    graph.write_pdf("iris.pdf")    
    
    return clf

def doDecisionTreeRegressor():

    y = df[0].values
    X = df[2].values
    regr_1 = DecisionTreeRegressor(max_depth=2)
    regr_2 = DecisionTreeRegressor(max_depth=5)

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
    
    regr_1.fit(X_train, y_train)
    regr_2.fit(X_train, y_train)

    y_1 = regr_1.predict(X_test)
    y_2 = regr_2.predict(X_test)

    # Plot the results
#    plt.figure(figsize=(8,6))
    plt.scatter(X_test, y_1, c="darkorange", label="data")
    plt.plot(X_test, y_test, color="cornflowerblue", label="max_depth=2", linewidth=2)
#    plt.plot(X_test, y_2, color="yellowgreen", label="max_depth=5", linewidth=2)
    plt.xlabel("data")
    plt.ylabel("target")
    plt.title("Decision Tree Regression")
    plt.legend()
    plt.show()


def visualize():
    # Create color maps
    cmap_light = ListedColormap(['#FFB0AA', '#FFE0AA', '#FFF4AA', '#F2FAA7', '#7AB793', '#748BA7'])
    cmap_bold = ListedColormap(['#550000', '#553D00', '#4B5300', '#004011', '#041F37', '#14073A'])

    h = .1  # step size in the mesh

    clf = neighbors.KNeighborsClassifier(6)
    clf.fit(X, y)

    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1

    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                         np.arange(y_min, y_max, h))
    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])

    # Put the result into a color plot
    Z = Z.reshape(xx.shape)
    plt.figure()
    plt.pcolormesh(xx, yy, Z, cmap=cmap_light)

    # Plot also the training points
    plt.scatter(X[:, 0], X[:, 1], c=y, cmap=cmap_bold,
                s=10)
    plt.xlim(xx.min(), xx.max())
    plt.ylim(yy.min(), yy.max())

    plt.show()


def get_model_quality(model):
    print("Training set score: {:.3f}".format(model.score(X_train, y_train)))
    print("Test set score: {:.3f}".format(model.score(X_test, y_test)))

    
def predict(file, model):

    filename, file_extension = os.path.splitext(file)

    if file_extension != '.gpx':
        stderr.write('Please enter a valid GPX file.')
        return

    gpx_file = open(file, 'r')

    gpx = gpxpy.parse(gpx_file)
    
    if len(gpx.tracks) < 1:
        return
    
    data = gpx.tracks[0
    ].get_moving_data()

    if (data.moving_time == 0 or data.max_speed == 0):
        return

    average_speed = data.moving_distance / data.moving_time
    max_speed = data.max_speed

    print(model.predict([[average_speed, max_speed]]))


def main():

    if len(sys.argv) == 1:
        #get_model_quality(KNF())
        #get_model_quality(RandomForest())
        get_model_quality(DecisionTree())
        #visualize()
        #doDecisionTreeRegressor()
        
    else:
        predict(sys.argv[1], KNF())
            
if __name__ == '__main__':
    main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
