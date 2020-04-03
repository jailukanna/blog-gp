---
title: matplotlib example pie (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pie'


Modules used in program: 
* `import math`
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import seaborn as sns`

## python pie

Python matplotlib example: pie

```python
import seaborn as sns
from sklearn import preprocessing
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from dask.optimization import inline
import matplotlib
matplotlib.style.use('ggplot')
import math
from wordcloud import WordCloud, STOPWORDS


# from IPython import get_ipython

# get_ipython().magic('matplotlib', 'inline')

df = pd.read_csv("appendix.csv", sep=',')

course_titles = df['Course Title']

fct = course_titles[0:8]

students_count = df['Participants (Course Content Accessed)']

sct = students_count[0:8]

colors = ["#1f77b4", "#ff7f0e", "#2ca02c", "#d62728", "#8c564b"]

plt.pie(sct, labels=fct, autopct='%1.1f%%', colors=colors)

plt.title("Percentage of the students taking ten particular courses")
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
