---
title: matplotlib example plotting (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plotting'


Modules used in program: 
* `import matplotlib`
* `import matplotlib.ticker as mticker`
* `import matplotlib.pyplot as plt`
* `import cartopy.crs as ccrs`

## python plotting

Python matplotlib example: plotting

```python
###############################################################################
# Panel operations
###############################################################################
# Draw a horizontal line at y = 0
ax.Axes.axhline(y=0)

# Text using axes coordinates, and fontdict to control appearances.
ax.text(0.1, 0.9, 'text', fontdict = {}, transform = ax.transAxes)

# Add gridlines and latitude & longitude coordinates to cartopy maps
# https://stackoverflow.com/questions/49956355/adding-gridlines-using-cartopy
import cartopy.crs as ccrs
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker
from cartopy.mpl.gridliner import LONGITUDE_FORMATTER, LATITUDE_FORMATTER

# ... Figure and plotting commands for other features of the map

# The key argument is draw_labels = True. The rest modifies the appearance of
# the GridLiner (gl) instance.
gl = ax.gridlines(crs=ccrs.PlateCarree(), linewidth=2, color='black', alpha=0.5,
                  linestyle='--', draw_labels=True)
gl.xlabels_top = False
gl.ylabels_left = False
gl.xlocator = mticker.FixedLocator([120, 140, 160, 180, -160, -140, -120])
gl.ylocator = mticker.FixedLocator([0, 20, 40, 60])
gl.xformatter = LONGITUDE_FORMATTER
gl.yformatter = LATITUDE_FORMATTER
gl.xlabel_style = {'color': 'red', 'weight': 'bold'}


###############################################################################
# Figure operations
###############################################################################
fig.savefig('filename.png', bbox_inches = 'tight')

###############################################################################
# Colormap & colorbar operations
###############################################################################
# Get a list of colors from colormap
import matplotlib
cmap = matplotlib.cm.get_cmap('Spectral')
rgba = cmap(0.5)
print(rgba) # (0.99807766255210428, 0.99923106502084169, 0.74602077638401709, 1.0)

# Use a list of colors to create a colormap
# https://matplotlib.org/tutorials/colors/colormap-manipulation.html#sphx-glr-tutorials-colors-colormap-manipulation-py
from matplotlib import colors, cm
viridis = cm.get_cmap('viridis', 256)
newcolors = viridis(np.linspace(0, 1, 256))
pink = np.array([248/256, 24/256, 148/256, 1])
newcolors[:25, :] = pink
newcmp = ListedColormap(newcolors)

# Transform the mapping of colormap to values
from matplotlib import colors
# (1) linear colormap
norm = colors.Normalize(vmin = -1., vmax = 1.)
# (2) normalize the colormap by log10, suitable for log-normally distributed data
norm = colors.LogNorm(vmin = 1e-6, vmax = 1.)
# (3) normalize the colormap by log10 on either side of the axis, suitable for data that is
# log-normal on both the positive and negative sides of the axis
# ---- linthresh controls the part for which to keep the colormap linear
#      linscale controls the length of the linear part
norm = colors.SymLogNorm(linthresh=0.03, linscale=0.03, vmin=-1.0, vmax=1.0)
# (4) normalize the data using power law: when gamma < 1, small numbers are more
# resolved, large numbers are less resolved, like the log-transformation
norm = colors.PowerNorm(gamma = 0.5)
# (5) "boundary normalization": specify the boundaries for value intervals. Values within the
#     same interval will be mapped to the same color
bounds = np.array([-0.25, -0.125, 0, 0.5, 1])
norm = colors.BoundaryNorm(boundaries=bounds, ncolors=256) # returns values from 0 to ncolors-1 instead of [0,1]
# (6) ... can define your own class of normalization by inheriting from colors.Normalize. Too complex for me now.
# 
# (example)
N = 100
X, Y = np.mgrid[-3:3:complex(0, N), -2:2:complex(0, N)]
Z1 = np.exp(-X**2 - Y**2)
Z2 = np.exp(-(X - 1)**2 - (Y - 1)**2)
Z = (Z1 - Z2) * 2
fig, ax = plt.subplots()
pcm = ax.pcolormesh(X, Y, Z, norm=norm, cmap='RdBu_r')
fig.colorbar(pcm, ax=ax, extend='both', orientation='vertical', pad = 0.1)
# Or define a new axis for the colorbar
cax = fig.add_axes([0.92, 0.1, 0.02, 0.8])
fig.colorbar(pcm, cax = cax, boundaries = bounds)

###############################################################################
# Legend operations
###############################################################################
# Create a legend
artist_handles = ax.get_children()[0:5] # get the plot element from the graph
lgd = ax.legend(artist_handles, labels = ['1','2','3','4','5','6'], 
                frameon = False, loc = ['upper right', 'lower left', 'right', 'best', ...], 
                bbox_to_anchor = (1.1, 0.22), fontsize = 9, ncol = 2, 
                labelspacing = 0.1, # the vertical space between the legend entries 
                handlelength = 0.1, # the length of the legend handles, measured in fontsize units
                handletextpad = 0.1, # the pad between the legend handle and text
                columnspacing = 0.1) # the spacing between columns of the legend

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
