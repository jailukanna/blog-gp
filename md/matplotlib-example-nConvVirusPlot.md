---
title: matplotlib example nConvVirusPlot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'nConvVirusPlot'

Functions in program: 
* `def plot_daily():`
* `def catch_data():`

Modules used in program: 
* `import matplotlib.dates as mdates`
* `import matplotlib.pyplot as plt`
* `import matplotlib.figure`
* `import matplotlib`
* `import numpy as np`
* `import time, json, requests`

## python nConvVirusPlot

Python matplotlib example: nConvVirusPlot

```python
import time, json, requests
from datetime import datetime
import numpy as np
import matplotlib
import matplotlib.figure
from matplotlib.font_manager import FontProperties
from matplotlib.backends.backend_agg import FigureCanvasAgg
from matplotlib.patches import Polygon
from matplotlib.collections import PatchCollection
import matplotlib.pyplot as plt
import matplotlib.dates as mdates


def catch_data():

    data = dict()
    url = "https://view.inews.qq.com/g2/getOnsInfo?name=disease_h5&callback=&_=%d"%int(time.time()*1000)

    data = json.loads(requests.get(url=url).json()['data'])
    ChinaDayList = list()
    CDdate_list = list()
    CDconfirm_list = list()
    CDsuspect_list = list()
    CDdead_list = list()
    CDheal_list = list()

    ChinaDayAddList = list()
    CDAdate_list = list()
    CDAconfirm_list = list()
    CDAsuspect_list = list()
    CDAdead_list = list()
    CDAheal_list = list()

    ChinaDayList = data['chinaDayList']
    ChinaDayAddList = data['chinaDayAddList']
    for item in ChinaDayList:
        month, day = item['date'].split('.')
        CDdate_list.append(datetime.strptime('2020-%s-%s'%(month, day), '%Y-%m-%d'))
        CDconfirm_list.append(int(item['confirm']))
        CDsuspect_list.append(int(item['suspect']))
        CDdead_list.append(int(item['dead']))
        CDheal_list.append(int(item['heal']))

    for item in ChinaDayAddList:
        month, day = item['date'].split('.')
        CDAdate_list.append(datetime.strptime('2020-%s-%s' % (month, day), '%Y-%m-%d'))
        CDAconfirm_list.append(int(item['confirm']))
        CDAsuspect_list.append(int(item['suspect']))
        CDAdead_list.append(int(item['dead']))
        CDAheal_list.append(int(item['heal']))
    return CDdate_list, CDconfirm_list, CDsuspect_list, CDdead_list, CDheal_list, CDAdate_list, CDAconfirm_list, CDAsuspect_list, CDAdead_list, CDAheal_list

def plot_daily():
    CDdate_list, CDconfirm_list, CDsuspect_list, CDdead_list, CDheal_list, CDAdate_list, CDAconfirm_list, CDAsuspect_list, CDAdead_list, CDAheal_list = catch_data()


    plt.figure("2019-nConv Chart", facecolor='#f4f4f4', figsize=(10,8))

    plt.subplot(211)
    plt.title("Accumlation by Day plot", fontdict=None, loc='center', pad=None)
    plt.plot(CDdate_list, CDconfirm_list, label='day confirm')
    plt.plot(CDdate_list, CDsuspect_list, label='day suspect')
    plt.plot(CDdate_list, CDdead_list, label='day death')
    plt.plot(CDdate_list, CDheal_list, label='day heal')
    plt.gca().xaxis.set_major_formatter(mdates.DateFormatter('%m-%d'))
    plt.gcf().autofmt_xdate()
    plt.grid(linestyle=':')
    plt.legend(loc='best')


    plt.subplot(212)
    plt.title("Daily added cases", fontdict = None, loc='center', pad=None)
    plt.plot(CDAdate_list, CDAconfirm_list, label='dayAdd confirm')
    plt.plot(CDAdate_list, CDAsuspect_list, label='dayAdd suspect')
    plt.plot(CDAdate_list, CDAdead_list, label='dayAdd death')
    plt.plot(CDAdate_list, CDAheal_list, label='dayAdd heal')

    plt.gca().xaxis.set_major_formatter(mdates.DateFormatter('%m-%d'))
    plt.gcf().autofmt_xdate()
    plt.grid(linestyle=':')
    plt.legend(loc='best')

    plt.show()


if __name__ == '__main__' :
    plot_daily()
    # plot_distribution()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
