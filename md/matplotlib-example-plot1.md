---
title: matplotlib example plot1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot1'


Modules used in program: 
* `import pandas_datareader.data as web`
* `import plotly.graph_objs as go`
* `import plotly.plotly as py`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`

## python plot1

Python matplotlib example: plot1

```python
ipython -pylab
import matplotlib.pyplot as plt

fig=plt.figure()
plt.plot(1,1,1)

from matplotlib.finance import candlestick_ohlc # 蜡烛图

 a.xaxis_date() #a的x轴为日期
plt.show()

每次编写代码时进行参数设置
1，显示中文
#coding:utf-8
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif']=['SimHei'] #用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False #用来正常显示负号
#有中文出现的情况，需要u'内容'

2，时间转换为数字
先从unicode str转换成datetime
然后再实现
from matplotlib.pylab import date2num
···
组合成为一个list
zip()

3,构建画板并设置x轴为日期
fig, ax=plt.subplots()
```旋转日期
plt.xtics(rotation=45)


···
pplotly

import plotly.plotly as py
import plotly.graph_objs as go
import pandas_datareader.data as web

from datetime import datetime

df = web.DataReader("aapl", 'yahoo', datetime(2007, 10, 1), datetime(2009, 4, 1))

trace = go.Candlestick(x=df.index,
                       open=df.Open,
                       high=df.High,
                       low=df.Low,
                       close=df.Close)
data = [trace]
py.iplot(data, filename='simple_candlestick')

###https://plot.ly/python/candlestick-charts/




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
