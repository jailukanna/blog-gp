---
title: matplotlib example barchart 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'barchart 1'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib.dates as mdates`
* `import matplotlib.pyplot as plt`

## python barchart 1

Python matplotlib example: barchart 1

```python
%matplotlib inline
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import pandas as pd
import numpy as np

#load datafile
url1 = 'https://sabopy.com/py/barchart_1'
data = pd.read_html(url1,header=0)[0]
data
'''
Date	Score
0	2018-09-23	6418
1	2018-09-24	16065
2	2018-09-25	5201
3	2018-09-26	8995
4	2018-09-27	22515
5	2018-09-28	8436
6	2018-09-29	13638
'''

Date = data["Date"].values.astype(np.datetime64)
Date
'''
array(['2018-09-23', '2018-09-24', '2018-09-25', '2018-09-26',
       '2018-09-27', '2018-09-28', '2018-09-29'], dtype='datetime64[D]')
'''

Score = data['Score'].values
Score
#array([ 6418, 16065,  5201,  8995, 22515,  8436, 13638])

from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()

plt.rcParams["font.size"] = 14

fig = plt.figure(figsize=(6, 4))
ax = fig.add_subplot(111)
ax.bar(Date,Score,color = 'pink',edgecolor="black")
ax.xaxis.set_major_formatter(mdates.DateFormatter('%m/%d'))
ax.set_ylabel("Score") # y軸    
plt.savefig("Date_Score.png", dpi=120,transparent = False, bbox_inches = 'tight', pad_inches = 0.05)
plt.show()
 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
