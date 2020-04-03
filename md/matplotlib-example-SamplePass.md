---
title: matplotlib example SamplePass (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'SamplePass'


Modules used in program: 
* `import matplotlib.patches as mpatches`
* `import matplotlib`
* `import math`
* `import matplotlib.pyplot as plt`
* `import matplotlib.ticker as mtick`
* `import pandas`
* `import numpy as np`

## python SamplePass

Python matplotlib example: SamplePass

```python
import numpy as np
import pandas
import matplotlib.ticker as mtick

import matplotlib.pyplot as plt
import math
import matplotlib
import matplotlib.patches as mpatches



from matplotlib import rc
rc('font',**{'family':'sans-serif','sans-serif':['Helvetica']})
rc('text', usetex=True)

data = pandas.read_csv('file_Efull_0.txt',header = None,delimiter=r"\s+")
shn_data = pandas.read_csv('shn_intens.dat',header = None)
data1 = pandas.read_csv('file_Efull_1.txt',header = None,delimiter=r"\s+")
shn_data2 = pandas.read_csv('shn_intens2.dat',header = None)

l = len(data.axes[0]) + 1000
x = []
y = []
y2 = []
#for i in range(5) :
#    x.append(i/100-5)
#    y.append( data[0][0])

for i in range(l-1000) :
    x.append((i)/100)
    y.append( data[2][i]*data[2][i]*data[0][i]/data[0][0])

#for i in range(l-1000) :
#    y2.append( data1[5][i]*data1[5][i]*data1[0][i]/data1[0][0])

#for i in range(l-500,l,1) :
#    x.append(i/100)
#    y.append( data[0][len(data.axes[0])-1])

ax = plt.figure().add_subplot(111)
ax.yaxis.set_major_formatter(mtick.FormatStrFormatter('%.4e'))
#ax.plot(x1,y1,c='g', label=r'$\\2.5*10^{15}W/m^2')

#ax.plot(x,y,c='b', label=r'$\\5.0*10^{15}W/m^2')
ax.plot(x,y,c='r', label=r'$\\Transmitted \ wave \ intensity$', linewidth=1.5)
#ax.plot(x,y2,c='r', label=r'$\\Layer \ Model, \Delta n_a = 2.88 \cdot 10^{-5}$', linewidth=0.3)
#ax.plot(shn_data[0],shn_data[1],'blue', label=r'$\\Analytical [28], \Delta n_a = 3.7 \cdot 10^{-5}$',linestyle='dashed', linewidth=2)
#ax.plot(shn_data2[0],shn_data2[1],'blue', label=r'$\\Analytical [28], \Delta n_a = 2.88 \cdot 10^{-5}$', linewidth=2) #linestyle='dashed',

#ax.plot(x3,y3,c='r', label=r'$\\1.0*10^{16}W/m^2')

#ax.hlines(y=3.693e-5,xmin=0,xmax=1.8e-9, colors='k',linestyles='dotted')
#ax.hlines(y=1.88e-5,xmin=0,xmax=1.8e-9, colors='k',linestyles='dotted')
#ax.hlines(y=7.36e-5,xmin=0,xmax=1.8e-9, colors='k',linestyles='dotted')
#ax.vlines(x=3.121e-10,ymin=0,ymax=9.e-5, colors='k',linestyles='dashed')


plt.legend( bbox_to_anchor=(0.89, 0.99),loc=1, borderaxespad=0.)
#plt.ylim(0.0, 1.e-3)
#plt.xlim(0, 400)
plt.xlabel(r'$x/\lambda$',fontsize=14)
plt.ylabel(r'$I_{+}(x) / I_{+}(0)$',fontsize=16)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
