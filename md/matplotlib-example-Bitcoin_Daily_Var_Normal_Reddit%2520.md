---
title: matplotlib example Bitcoin Daily Var Normal Reddit%2520 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Bitcoin Daily Var Normal Reddit%2520'

Functions in program: 
* `def init() :`
* `def update(*args) :`
* `def grafico(data, leg1) : `

Modules used in program: 
* `import quandl`
* `import matplotlib.animation as animation`
* `import matplotlib`
* `import seaborn as sns`
* `import numpy as np`

## python Bitcoin Daily Var Normal Reddit%2520

Python matplotlib example: Bitcoin Daily Var Normal Reddit%2520

```python
# -*- coding: utf-8 -*-
from matplotlib import pyplot
import numpy as np
import seaborn as sns
import matplotlib
import matplotlib.animation as animation
import quandl

quandl.ApiConfig.api_key = '????????????????????' # Here goes your key - get it on Quandl´s website after you open an free account.
                                                  # www.quandl.com




p0 = '20150101'
p1 = '20181231'

data1 = quandl.get("BNC3/GWA_BTC")[p0:p1] # Data BraveNewCoin Daily Global Price Index for Bitcoin.

var = ((data1["Close"]/data1["Open"])-1)*100

data1["Var"] = list(var)





fig, g = pyplot.subplots(figsize=(8.76,6.9))
pyplot.style.use("ggplot")
pyplot.subplots_adjust(top = 0.92, hspace = 0.20, bottom = 0.06,left = 0.06, right = 0.94)

z = "wheat" 
pyplot.rcParams['savefig.facecolor'] = z


matplotlib.rcParams['xtick.labelsize'] = 8.8 
matplotlib.rcParams['ytick.labelsize'] = 8.8 



def grafico(data, leg1) : 

   
    #Top chart
    pyplot.subplot2grid((3,1), (0,0), rowspan=2, colspan=1)
    pyplot.title(leg1,fontsize = 9, loc = 'right')
    sns.distplot(data["Var"], kde=False, rug = True, hist = True,color = "steelblue",axlabel = False)
    
    #Bottom chart
    pyplot.subplot2grid((3,1), (2,0), rowspan=2, colspan=1)
    sns.boxplot(data = data["Var"],orient = "h",saturation = 0.8, fliersize = 6, palette = "Blues",linewidth = 0.95)
    sns.swarmplot( y="Var", data = data, size=2.5, color="orange", linewidth=0.850, alpha = 0.55,orient = "h")
    pyplot.xlabel("%", fontsize = 8)
    return()
    



ctd = 60


def update(*args) :
  
  global data1, ctd, dts,lgd
  
  dts = data1[0:ctd]
  
  ctd +=60       
 
  
  pyplot.clf()
  
  lgd1 = "From : " + str(dts.index[0])[0:10] + "  to : " + str(dts.index[-1])[0:10] + " -  total days : " + str(len(dts)) 
  grafico(dts,lgd1)
  pyplot.suptitle(" " + " Bitcoin, daily % variations", fontsize = 10.5)   #fs = 13  
    
  return ()




def init() :
    
  return ()


anim = animation.FuncAnimation(fig, update, init_func=init, blit = False, frames=26, repeat = False)


pyplot.show()





sf = "BTC_daily_var" +".gif"      
Sname = "C:\Users\Vader\Desktop\_" + sf   # The directory you may choose to save your gif file - Change it !!!
         
# You must have imagemagick installed in you computer :
# https://www.imagemagick.org/script/index.php
#
# Mine is istalled in C:\program files\  
   
anim.save(Sname, writer="imagemagick", fps=2)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
