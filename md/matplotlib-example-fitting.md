---
title: matplotlib example fitting (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'fitting'

Functions in program: 
* `def f_show(x,p_fit):`
* `def f_fit(x,a,b,c):`
* `def f(x):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import  scipy as sp`

## python fitting

Python matplotlib example: fitting

```python
# 多项式拟合
x = np.arange(9) 
y = df_pro['SUM(A."COUNT")'].as_matrix()

import  scipy as sp
fp =sp.polyfit(x,y,3)  
f=sp.poly1d(fp)
fx=sp.linspace(0,x.max(),100)

import matplotlib.pyplot as plt
plt.plot(fx,f(fx),linewidth=4, color='r')
plt.plot(x,y,linewidth=4, color='b')
plt.legend(["d=%i" %f.order], loc="upper left")

import numpy as np
from matplotlib import pyplot as plt
from scipy.optimize import curve_fit
 
def f(x):
    return np.exp(1.5*x+2)-1
def f_fit(x,a,b,c):
    return np.exp(a*x+b)+c
def f_show(x,p_fit):
    a,b,c=p_fit.tolist()
    return np.exp(a*x+b)+c

x=np.linspace(-2,2)
y=f(x)+1*np.random.randn(len(x))#加入了噪音
p_fit,pcov=curve_fit(f_fit,x,y)#曲线拟合
print(p_fit)#最优参数
print(pcov)#最优参数的协方差估计矩阵
y1=f_show(x,p_fit)
plt.plot(x,f(x),'r',label='original')
plt.scatter(x,y,c='g',label='before_fitting')#散点图
plt.plot(x,y1,'b--',label='fitting')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
