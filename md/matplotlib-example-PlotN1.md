---
title: matplotlib example PlotN1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'PlotN1'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python PlotN1

Python matplotlib example: PlotN1

```python
import matplotlib.pyplot as plt
import numpy as np


X = np.arange(1,91)

Y = np.sin(X*np.pi/180)

Y2 = np.cos(X*np.pi/180)

print((X))
print((Y))

#Mark 30,45,60,90 degrees on the curve. ha , va shortcut for horizontal alignment, vertical alignment.
for i in (30,45,60,90):
 plt.text(i,np.sin(i*np.pi/180) ,str(i) + ' degrees', va ='top', ha = 'left')

plt.plot(X,Y)

plt.show()


#using annotate to construct an arrow pointer in the curve to represent various values.
plt.annotate('Mid point of curve',
             xytext = (40,np.sin(30*np.pi/180) ),
             xy = (45,np.sin(45*np.pi/180) ),
arrowprops = { 'facecolor' : 'black','shrink' : 0.01  })
             #)
plt.plot(X,Y)

plt.show()


#use legend to provide the details of each plot .
plt.plot(X,Y,'g', label = 'SIN(x)', linewidth = 3.)
plt.plot(X,Y2,'r--', label = 'COS(x)', linewidth = 2.)

plt.legend(loc='center', title = 'Type of curve', fancybox =True)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
