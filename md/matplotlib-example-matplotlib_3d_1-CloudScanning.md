---
title: matplotlib example matplotlib 3d 1-CloudScanning (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib 3d 1-CloudScanning'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python matplotlib 3d 1-CloudScanning

Python matplotlib example: matplotlib 3d 1-CloudScanning

```python

import matplotlib.pyplot as plt
from matplotlib import colors
import numpy as np
data = np.loadtxt(open("data5.csv", "rb"), delimiter=",", skiprows=0)#读取矩阵数据
data=data[:,0:600]#取前600列，即3000m
print(data.shape)
rotation_angle_from=30#绘图起始角度
rotation_angle_to=150#绘图结束角度
distance_from=0#开始角度
data_tick=5 #测量间隔，5m

distance_to=data.shape[1]*data_tick
radians = np.radians(np.linspace(rotation_angle_from, rotation_angle_to, data.shape[0]))#可由第一列数据或某一列数据生成arnge。
zeniths = np.arange(distance_from, distance_to, data_tick)
r, theta = np.meshgrid(zeniths, radians)
#主图绘制
fig, ax = plt.subplots(subplot_kw=dict(projection='polar'))

norm=colors.Normalize(vmin=0,vmax=2,clip=True)


cmap = plt.get_cmap('rainbow')

cf=ax.contourf(theta, r, data,cmap=cmap,norm=norm,levels=[0,0.25,0.5,0.75,1,1.25,1.5,1.75,2])
ax.set_axis_off()
#绘制图例
cbar = fig.colorbar(cf,ax=ax)
# cbar.set_ticks(np.linspace(0, 2, 9 ))
plt.rcParams['savefig.dpi'] = 1200 #图片像素300,600,1200等
plt.rcParams['figure.dpi'] = 1200 #分辨率300,600,1200等
plt.savefig('plot_show.png')#保存

plt.show()#展示


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
