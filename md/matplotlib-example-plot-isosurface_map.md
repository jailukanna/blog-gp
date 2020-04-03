---
title: matplotlib example plot-isosurface map (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot-isosurface map'


Modules used in program: 
* `import matplotlib.cbook as cbook`
* `import scipy.interpolate`
* `import matplotlib.mlab as mlab`
* `import matplotlib.cm as cm`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`

## python plot-isosurface map

Python matplotlib example: plot-isosurface map

```python
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.cm as cm

import matplotlib.mlab as mlab
from matplotlib.mlab import griddata
import scipy.interpolate


matplotlib.rcParams['xtick.direction'] = 'out'
matplotlib.rcParams['ytick.direction'] = 'out'


# setup resolution
numcols, numrows = 1000, 1000

# setup map dimessions - area2 google
loni = 136.16948
lonf = 136.24072
lati= 35.78712
latf = 35.85836

# get data of df DATA["x","y","ppt"]
xi = np.linspace(loni, lonf, numcols)
yi = np.linspace(lati, latf, numrows)
# build grid
xi, yi = np.meshgrid(xi, yi)
npz = np.asarray(DATA["ppt"])
x, y, z = DATA.x.values, DATA.y.values, npz
zi = griddata(x, y, z, xi, yi, interp='nn')


# setup figure
my_dpi = 113
fig = plt.figure(figsize=(1500/my_dpi, 1500/my_dpi), dpi=my_dpi)
ax = fig.add_subplot(111)

# levels
levels = np.arange(0, int(npz.max())+1, step(0,int(npz.max())+1))


# Interpolate linear
rbf = scipy.interpolate.Rbf(x, y, z, function='linear') # Also possible: cubic, gaussian, inverse_multiquadric, linear, multiquadric, quintic, thin_plate
zi = rbf(xi, yi)
# Interplation nearest neighbour
#zi = griddata(x, y, z, xi, yi, interp='nn')


######## SETUP PICTURE BACKGROUND
import matplotlib.cbook as cbook
from scipy.misc import imread
path_file = os.path.join(folder_img,file_img)
img = imread(path_file)
plt.imshow(img,zorder=0,extent=[loni,lonf, lati,latf])


######## PLOT ISO SURFACE
plt.imshow(zi, vmin=z.min(), vmax=z.max(), origin='lower',cmap=cm.jet,
           extent=[loni, lonf, lati, latf],alpha=0.6)

# Set color bar isosurface    
cbar = plt.colorbar(orientation='vertical',aspect=20,fraction=0.15,shrink=0.6, extend='both',ticks=levels)
cbar.set_label('ppt', rotation=0,fontsize='large',labelpad=-42, y=1.02)

######## PLOT ISO LINES
# levels
levels = np.arange(0, int(npz.max())+1, step(0,int(npz.max())+1))

# plot contour
CS = plt.contour(xi,yi,zi, levels=levels, hold='on',origin='lower',cmap=cm.jet,linewidths=1)

# plot labels of contour lines
plt.clabel(CS, inline=1,fmt='%1.1f',fontsize=12)

# make a colorbar for the contour lines
#CB = plt.colorbar(CS, orientation='vertical',aspect=20,fraction=0.15,shrink=0.6, extend='both',ticks=levels)



######## SCATTER PLOT
plt.scatter(x, y, c=z)
# set labels of scatter points
for i, value in enumerate(list(DATA.index)):
    ax.annotate(value, (x[i]+0.0003,y[i]+0.0003),fontsize="9")


########  Set limits to chart
plt.xlim((loni,lonf))
plt.ylim((lati,latf))


######## Hide axis labels
plt.gca().xaxis.set_major_locator(plt.NullLocator())
plt.gca().yaxis.set_major_locator(plt.NullLocator())



## SAVE CHART
path_output = os.path.join(folder_output,"output-contour-juan2.png")
plt.savefig(path_output,dpi=my_dpi * 1,bbox_inches="tight") # if * 1 -> 1280×800
# clean plot
plt.clf()



# clean df of data
del(DATA)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
