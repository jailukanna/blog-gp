---
title: matplotlib example matplotlib 3d 1-CloudScanning bystep (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib 3d 1-CloudScanning bystep'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python matplotlib 3d 1-CloudScanning bystep

Python matplotlib example: matplotlib 3d 1-CloudScanning bystep

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
i=0
distance_to=data.shape[1]*data_tick
radians = np.linspace(i, i+2, 2)*np.pi/180
rs = np.arange(distance_from, distance_to, data_tick)
Rs, Thetas = np.meshgrid(rs, radians)
#主图绘制
fig, ax = plt.subplots(subplot_kw=dict(projection='polar'))
norm=colors.Normalize(vmin=0,vmax=2,clip=True)
cmap = plt.get_cmap('rainbow')

data_i=data[i:i+2,:]
print(data_i)
cf=ax.contourf(Thetas, Rs,data_i, cmap=cmap )
# ax.set_axis_off()
# #绘制图例
cbar = fig.colorbar(cf,ax=ax)
cbar.set_ticks(np.linspace(0, 2, 9 ))
plt.rcParams['savefig.dpi'] = 300 #图片像素300,600,1200等
plt.rcParams['figure.dpi'] = 300 #分辨率300,600,1200等
# plt.savefig('plot_show.png')#保存
plt.ion()
plt.show()#展示
while i<360:
	if i>0:
		data_i = data[i:i + 2, :]
		radians = np.linspace(i - 1, i + 1, 2) * np.pi / 180  # 改为line。。。。。
		Rs, Thetas = np.meshgrid(rs, radians)
		cmap = plt.get_cmap('rainbow')
		ax.contourf(Thetas, Rs, data_i, cmap=cmap)
	i=i+2
	plt.pause(0.1)
plt.savefig('stepshow.png')#保存

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
