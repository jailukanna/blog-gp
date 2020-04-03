---
title: matplotlib example importing listing (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'importing listing'


Modules used in program: 
* `import folium`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import numpy as np`
* `import geopandas as geopd`
* `import pandas as pd`

## python importing listing

Python matplotlib example: importing listing

```python
# for the analysis
import pandas as pd
import geopandas as geopd
import numpy as np

# for the charts
import seaborn as sns
import matplotlib.pyplot as plt

# for the map
import folium
from folium.plugins import MarkerCluster
from folium import IFrame

# for Jupyter notebook display
%matplotlib inline

calendar_df = pd.read_csv('calendar.csv.gz', compression='gzip')
listings_df = pd.read_csv('listings.csv.gz', compression='gzip')
reviews_df = pd.read_csv('reviews.csv.gz', compression='gzip')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
