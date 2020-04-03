---
title: matplotlib example kline (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'kline'


Modules used in program: 
* `import datetime`
* `import matplotlib.ticker as mticker`
* `import matplotlib.dates as mdates`
* `import matplotlib.pyplot as plt`

## python kline

Python matplotlib example: kline

```python
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import matplotlib.ticker as mticker
from matplotlib.finance import candlestick_ohlc
import datetime
from matplotlib.dates import date2num

fig = plt.figure()
ax1 = plt.subplot2grid((1,1), (0,0))

ohlc = [[date2num(datetime.datetime.fromtimestamp(k[0]/1000)), k[1], k[2], k[3], k[4]] for k in kline]
#pprint.pprint(ohlc)
#ohlc = [[date2num(datetime.datetime.fromtimestamp(1517328000)), 10289.1, 10425, 9390.69, 9931]]
candlestick_ohlc(ax1, ohlc, width=0.4, colorup='#77d879', colordown='#db3f3f')

for label in ax1.xaxis.get_ticklabels():
    label.set_rotation(45)

ax1.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
ax1.xaxis.set_major_locator(mticker.MaxNLocator(10))
ax1.grid(True)


plt.xlabel('Date')
plt.ylabel('Price')
plt.title('stock')
plt.legend()
plt.subplots_adjust(left=0.09, bottom=0.20, right=0.94, top=0.90, wspace=0.2, hspace=0)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
