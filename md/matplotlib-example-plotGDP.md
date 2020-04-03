---
title: matplotlib example plotGDP (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plotGDP'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python plotGDP

Python matplotlib example: plotGDP

```python
import matplotlib.pyplot as plt
import numpy as np

Countries = [
'United States',
'China',
'Japan',
'Germany',
'United Kingdom',
'India',
'France',
'Brazil',
'Italy',
'Canada',
'Russia',
'South Korea'
]

Countries_arr = np.arange(len(Countries))
GDP = np.array([
19417.14,
11795.30,
4841.22,
3423.29,
2496.76,
2454.46,
2420.44,
2140.94,
1807.43,
1600.27,
1560.71,
1498.07
])

#plot GDP of countries

plt.bar(Countries_arr,GDP)
plt.xticks(Countries_arr, Countries)
plt.show()

#Pie chart with custom Html colours.
Colourcode = ['#FF0000',
'#800000',
'#FFFF00',
'#808000',
'#00FF00',
'#008000',
'#00FFFF',
'#008080',
'#0000FF',
'#000080' ]
plt.pie(GDP,labels=Countries,autopct='%1.1f%%', colors=Colourcode)
plt.show()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
