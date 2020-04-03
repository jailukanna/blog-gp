---
title: matplotlib example PlotN5 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'PlotN5'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python PlotN5

Python matplotlib example: PlotN5

```python
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
ax1 = fig.add_subplot(2,2,1)
ax2 = fig.add_subplot(2,2,2)
ax3 = fig.add_subplot(2,2,3)

X = np.arange(1,90)
Y = np.sin(X)
Y1 = np.cos(X)
Y2 = np.tan(X)

ax1.plot(X,Y,'-')
ax2.plot(X,Y1,'-')
ax3.plot(X,Y2,'-')

plt.show()


#get the figure and Axes object from a 2x2 subplot
fig, Ax = plt.subplots(2,2)

#plot various figures using the functions available under Axes object. you can use a for loop and range.
Ax[0,0].bar(X,Y)
Ax[0,1].scatter(X,Y)
Ax[1,0].hist(Y, bins=45)
Ax[1,1].pie(X,Y)  #This is something different.
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
