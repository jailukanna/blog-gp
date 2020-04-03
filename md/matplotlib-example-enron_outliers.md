---
title: matplotlib example enron outliers (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'enron outliers'


Modules used in program: 
* `import matplotlib.pyplot`
* `import sys`
* `import pickle`

## python enron outliers

Python matplotlib example: enron outliers

```python
#!/usr/bin/python

import pickle
import sys
import matplotlib.pyplot
sys.path.append("C:\Users\USER\Desktop\machinelearning\ud120-projects-master/tools/")
from feature_format import featureFormat, targetFeatureSplit


### read in data dictionary, convert to numpy array
data_dict = pickle.load( open("C:\Users\USER\Desktop\machinelearning\ud120-projects-master/final_project/final_project_dataset.pkl", "r") )
data_dict.pop('TOTAL', 0) # column 'TOTAL' is included in data points
features = ["salary", "bonus"]
data = featureFormat(data_dict, features)

from pprint(import pprint)
outliers = []
for key in data_dict:
    val = data_dict[key]["salary"]
    if val == 'NaN':
        continue
    outliers.append((key, val))
outliers.sort(key=lambda x: x[1], reverse=True)
print(outliers[:2])

### your code below




####### visualize a scattor plot #######
for point in data:
    salary = point[0]
    bonus = point[1]
    matplotlib.pyplot.scatter( salary, bonus )

matplotlib.pyplot.xlabel("salary")
matplotlib.pyplot.ylabel("bonus")
matplotlib.pyplot.show()
########################################

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
