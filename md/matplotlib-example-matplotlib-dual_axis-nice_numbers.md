---
title: matplotlib example matplotlib-dual axis-nice numbers (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib-dual axis-nice numbers'

Functions in program: 
* `def top_tick_function_inv(r, target_value=0): `
* `def top_tick_function(p): return sc.h*sc.c/(p*sc.nano)/sc.eV`
* `def right_tick_function_inv(r, target_value=0): `
* `def right_tick_function(p): return 10.46*(p**1.68)`

Modules used in program: 
* `import matplotlib.gridspec as gridspec`
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import scipy.constants as sc`
* `import numpy as np`

## python matplotlib-dual axis-nice numbers

Python matplotlib example: matplotlib-dual axis-nice numbers

```python
#!/usr/bin/python3
#-*- coding: utf-8 -*-


import numpy as np
import scipy.constants as sc

import matplotlib.pyplot as plt
import matplotlib


## Ordinary plotting code
fig = plt.figure(figsize=(7,4), dpi=96, facecolor='#eeeeee', tight_layout=1)
ax = fig.add_subplot(121)
#XXX ax2.axis['top'].major_ticklabels().set_visible(False)
#XXX ax2.axis['top'].major_ticklabels.set_visible(False)
#XXX TypeError: 'method' object is not subscriptable


import matplotlib.gridspec as gridspec
gs = gridspec.GridSpec(1, 2, width_ratios=[.95, .05] )
ax, axR = plt.subplot(gs[0]), plt.subplot(gs[1])
print(ax)

#from   mpl_toolkits.axes_grid1 import host_subplot
#ax = host_subplot(111)
#print(ax)

#hax = host_axes([0.1,0.1,0.8,0.8], figure=fig)
#hax.plot([0,1,2,3],[1,2,4,8])
#fig.savefig("some_file.png")


x = np.arange(1.6, 3.2, .1)
p = np.arange(2.0, 11.0, .1)
xs, ps = np.meshgrid(x, p)
zs = 1/(1 + (xs-3)**4 + (ps-5)**2)
contourf = ax.contourf(x,p,zs, cmap=matplotlib.cm.viridis)

#x = np.arange(1.6, 3.2, 0.1)
#ax.plot(x, 6+4*np.sin(x*np.pi*2))
ax.set_title('Cathodoluminescence spectra\n\n\n')
ax.set_xlabel('photon energy (eV)')
ax.set_xlim((1.6, 3.2))
ax.set_ylabel('electron acceleration voltage (kV)')


## Here we define the desired nonlinear dependence between dual y-axes:
def right_tick_function(p): return 10.46*(p**1.68)
ax2 = ax.twinx()
#ax2.axis['right'].major_ticklabels.set_visible(False)
ax2.set_ylim(np.array(ax.get_ylim()))
ax2.set_ylabel('electron penetration depth (nm)')
## If we wish nice round numbers on the secondary axis ticks...
right_ax_limits = sorted([right_tick_function(lim) for lim in ax.get_ylim()])
yticks2 = matplotlib.ticker.MaxNLocator(nbins=8, steps=[1,2,5]).tick_values(*right_ax_limits)
## ... we must give them correct positions. To do so, we need to numerically invert the tick values:
from scipy.optimize import brentq
def right_tick_function_inv(r, target_value=0): 
    return brentq(lambda r,q: right_tick_function(r) - target_value, ax.get_ylim()[0], ax.get_ylim()[1], 1e6)
valid_ytick2loc, valid_ytick2val = [], []
for ytick2 in yticks2:
    try:
        valid_ytick2loc.append(right_tick_function_inv(0, ytick2))
        valid_ytick2val.append(ytick2)
    except ValueError:      ## (skip tick if the ticker.MaxNLocator gave invalid target_value to brentq optimization)
        pass
## Finally, we set the positions and tick values
ax2.set_yticks(valid_ytick2loc)
from matplotlib.ticker import FixedFormatter
ax2.yaxis.set_major_formatter(FixedFormatter(["%g" % righttick for righttick in valid_ytick2val]))



## The same approach for the x-axes (relation between photon energy and vavelength)
def top_tick_function(p): return sc.h*sc.c/(p*sc.nano)/sc.eV
ax3 = ax.twiny()
#ax2.axis['top'].major_ticklabels.set_visible(False)
ax3.set_xlim(np.array(ax.get_xlim()))
ax3.set_xlabel('photon wavelength (nm)')
## If we wish nice round numbers on the secondary axis ticks...
print(ax.get_xlim())
print(top_tick_function(1.0))
print([top_tick_function(lim) for lim in ax.get_xlim()])
top_ax_limits = sorted([top_tick_function(lim) for lim in ax.get_xlim()])
print(top_ax_limits)
xticks2 = matplotlib.ticker.MaxNLocator(nbins=8, steps=[1,2,5]).tick_values(*top_ax_limits)
## ... we must give them correct positions. To do so, we need to numerically invert the tick values:
from scipy.optimize import brentq
def top_tick_function_inv(r, target_value=0): 
    return brentq(lambda r,q: top_tick_function(r) - target_value, ax.get_xlim()[0], ax.get_xlim()[1], 1e6)
valid_xtick2loc, valid_xtick2val = [], []
for xtick2 in xticks2:
    try:
        valid_xtick2loc.append(top_tick_function_inv(0, xtick2))
        valid_xtick2val.append(xtick2)
    except ValueError:      ## (skip tick if the ticker.MaxNLocator gave invalid target_value to brentq optimization)
        pass
## Finally, we set the positions and tick values
ax3.set_xticks(valid_xtick2loc)
from matplotlib.ticker import FixedFormatter
ax3.xaxis.set_major_formatter(FixedFormatter(["%g" % toptick for toptick in valid_xtick2val]))


#axR = fig.add_subplot(122)
plt.colorbar(contourf, extend='both', cax=axR, ax=[ax,ax2,ax3]) ## todo: overlaps the right axis , fraction=0.07
axR.set_ylabel('relative intensity (a. u.)')

#plt.colorbar(contourf, shrink=0.8, extend='both')

#from mpl_toolkits.axes_grid1.inset_locator import inset_axes
#axins = inset_axes(ax2,
                   #width="5%",  # width = 10% of parent_bbox width
                   #height="50%",  # height : 50%
                   #loc=3,
                   #bbox_to_anchor=(1, 1.1, 1, .5),
                   #bbox_transform=ax2.transAxes,
                   #borderpad=0,
                   #)
#plt.colorbar(contourf, cax=axins, ticks=[0, 1])


## Finish the plot
ax.legend(prop={'size':10}, loc='upper right')
ax.grid()
fig.savefig('with_round_numbers_testcbar.png') 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
