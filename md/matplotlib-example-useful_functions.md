---
title: matplotlib example useful functions (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'useful functions'

Functions in program: 
* `def func(x0, ampli, center, width):                      # define the function to be used for fitting`
* `def func(x0, ampli, center, width):                      # define the function to be used for fitting`
* `def reasonable_ticks(a): `
* `def run_bash(cmd): `
* `def get_param(name): `
* `def get_param(file):`

Modules used in program: 
* `import scipy.fftpack`
* `import linalg as la`
* `import subprocess`
* `import sys`
* `import scipy`
* `import h5py`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib, sys, os, time`

## python useful functions

Python matplotlib example: useful functions

```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-

##                                .               S             C               SM
## F1    PREAMB                   Import          UseLatex      StartFig 
## F2    GET                      Load1D          Gen1D         Get2D
## F3    
## F4
## F5    PLOT                     Plot1D                        Contours
## F6
## F7    LAYOUT
## F8
## F9
## F10                          
## F11                          
## F12                          


## == 1. Initialization ==

## Import common moduli
import matplotlib, sys, os, time
import matplotlib.pyplot as plt
import numpy as np
from scipy.constants import c, hbar, pi

## Use LaTeX
matplotlib.rc('text', usetex=True)
matplotlib.rc('font', size=12)
matplotlib.rc('text.latex', preamble = '\usepackage{amsmath}, \usepackage{yfonts}, \usepackage{txfonts}, \usepackage{palatino}') # \usepackage{lmodern}, 
matplotlib.rc('font',**{'family':'serif','serif':['Computer Modern Roman, Times']})  ## select fonts
## LaTeX options
#matplotlib.rc('text.latex',unicode=True)   ## use accented letters

## Start figure + subplot 
fig = plt.figure(figsize=(10,10))
ax = plt.subplot(111, axisbg='w')
fig.subplots_adjust(left=.05, bottom=.05, right=.99, top=.99, wspace=.05, hspace=.05) ## (for interactive mode)

## === 2. Load data ===

## Load 1D curve
filename = "input.dat"
(x, y) = np.loadtxt(filename, usecols=list(range(2)), unpack=True)

## Load 2D array
filename = "input.dat"
z = np.loadtxt(filename, unpack=False)      ## load use unpack=True for transposed data
x = np.linspace(0, 1, z.shape[1])           ## define the dimension of data axes
y = np.linspace(0, 1, z.shape[0])  

## Load n-dimensional arrays from a HDF5 file
import h5py
h5file = h5py.File('input.h5', "r")
print("Found datasets:", h5file.keys())
data = np.array(h5file['ez'])
print("Loaded dataset with shape:", data.shape)
z = data
x = np.linspace(0, 1, z.shape[1])           ## define the dimension of data axes
y = np.linspace(0, 1, z.shape[0])  

## Load 2D image as numpy
import scipy
Et = scipy.misc.imread(sys.argv[1], flatten=1)


## === Generate data ===

## Generating 1D data
x = np.linspace(1., 2*np.pi, 1000)
y = np.sin(x) + np.sin(x*3)/3 + np.sin(x*5)/5

## Generate gridded data from a given 2D function (TODO)
plt.subplot(1, 1, 1, adjustable='box', aspect=1.0) ## ensure the plot region is a square
xs = np.linspace(0, 1, 100)
ys = np.linspace(0, 1, 100)
zs = []
for y in ys:
    z = np.exp(-(xs**2+y**2))               ## generate data for the points on grid
    zs.append(z)
zs = np.array(zs)

## Interpolate 2D grid from scattered data
interp_anisotropy = 1
from matplotlib.mlab import griddata
zi = griddata(x, y*interp_anisotropy, z, xi, yi*interp_anisotropy, interp='linear')


## === Other file operations ===

## Decide the filename to load data
filename = sys.argv[1] if len(sys.argv)>1 else 'input.dat'
if not os.path.isfile(filename): raise IOError, 'File %s can not be opened!' % filename

## Load header to the 'parameters' dictionary
def get_param(file):
    parameters = {}
    with open(filename) as datafile:
        for line in datafile:
            if (line[0:1] in '0123456789') or ('column' in line.lower()): break    # end of parameter list
            key, value = line.replace(',', ' ').split()[-2:]
            try: value = float(value) ## Try to convert to float, if possible
            except: pass                ## otherwise keep as string
            parameters[key] = value
    return parameters

## Parse the directory name for parameters (e.g. "Directory_x=3_comment=abcd" parses to the {'x':'3', 'comment':'abcd'} dictionary)
cwd_params = dict([x.split('=')    for x in os.getcwd().split('_')   if '=' in x])

## Sort arguments by a _numerical_ value in their parameter, keep the color order
import sys
filenames = sys.argv[1:]
def get_param(name): 

params  = [get_param(n) for n in filenames]
datasets = zip(params, filenames)                               ## sort the files by the parameter
datasets.sort()
colors = matplotlib.cm.hsv(np.linspace(0.1,0.9,len(filenames)+1)[:-1]) ## add the colors to sorted files
datasets = zip(colors, *zip(*datasets))
for color, param, filename in datasets:
    (x, y) = np.loadtxt(filename, usecols=[0,1], unpack=True)
    plt.plot(x,y/yref,color=color, label='$p = %.3f$'% (param))

## Run in shell
import subprocess
def run_bash(cmd): 
    return subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE).stdout.read().strip()


## === Data preprocess ===

## Truncate the data sets
mask = np.logical_and(freq>minf, freq<maxf)
(a, b, c, freq) = map(lambda arr: arr[mask], (a, b, c, freq))




## ===== Plotting 2D data ===== 
## Plot line
plt.plot(x, np.real(y), color="#FF8800", label=u"$y'$", ls='-')

## Plot multiple lines at once + use jet colormap
color = matplotlib.cm.hsv(float(number)/len(data_sources))

## Plot contours for gridded data
extent = max(-np.min(z), np.max(z))
contours = plt.contourf(x, y, np.abs(z), levels=np.linspace(-extent,extent,50), cmap=matplotlib.cm.RdBu, extend='both')
for contour in contours.collections: contour.set_antialiased(False)     ## optional: avoid white aliasing (for matplotlib 1.0.1 and older) 
plt.clabel(plt.contour(x, y, np.abs(z), levels=[0,0], colors='k'))      ## optional: plot black contour at zero
plt.colorbar() #.set_ticks([-1, -.25, 0, .25, 1])                        ## optional: colorbar

## Set up logarithmic contours (for z having both very small and very big positive values)
log_min, log_max = np.log10(np.min(z)), np.log10(np.max(z))
levels = 10**(np.arange(         log_min,          log_max,  .2))       ## where the contours are drawn
ticks  = 10**(np.arange(np.floor(log_min), np.ceil(log_max),  1))       ## where a number is printed
contours = plt.contourf(ki, fi, z, levels=levels, cmap=plt.cm.gist_earth, norm = matplotlib.colors.LogNorm())
plt.colorbar(ticks=ticks, ticklabels=['$10^{%d}$' % int(np.log10(t)) for t in ticks])

## Plot parametric curve that changes its colour along its length (e. g. for Smith chart)
si, co = np.sin(np.linspace(0,2*np.pi)), np.cos(np.linspace(0,2*np.pi))
plt.plot(si, co, a=.5);  plt.plot(si/2+.5, co/2, a=.3); plt.plot(si+1, co+1, a=.3); plt.plot(si+1, co-1, a=.3)
x = data.real; y = data.imag
t = np.linspace(0, 10, len(freq))
points = np.array([x, y]).T.reshape(-1, 1, 2)
segments = np.concatenate([points[:-1], points[1:]], axis=1)
lc = LineCollection(segments, cmap=plt.get_cmap('jet'), norm=plt.Normalize(0, 10))
lc.set_array(t); lc.set_linewidth(2)
plt.gca().add_collection(lc)


## ===== Subplot layout ===== 

## Adjacent subplots
ax.label_outer()        ## disable x-axis of subplot (if axis shared with the next subplots)

## Column span (whole height by default)
ax = plt.subplot2grid((rowcount, columncount), (0,1), rowspan=rowcount)

## Position of axis label
ax.yaxis.set_label_coords(-0.2, 0.1)


## ==== Outputting ====
## Finish the plot + save 
plt.xlabel(u"x"); 
plt.ylabel(u"y"); 
plt.grid()
plt.legend(prop={'size':10}, loc='upper right').draw_frame(False)
plt.savefig("output.png", bbox_inches='tight')

## SaveTxt 
with open('output.dat', 'w') as outfile: 
    outfile.write("#freq[THz]\t E(t)\n")
    np.savetxt(outfile, zip(x, y), fmt="%.8e")

## SaveTxt with header
# TODO


## ==== Plot limits and numbering ====
## Simple axes
plt.ylim((-0.1,1.1)); plt.yscale('linear')
plt.xlim((-0.1,1.1)); plt.xscale('linear')

## Logarithmic axes
plt.ylim((1e0, 1e2)); plt.yscale('log')
yticks = [2,5,10,20,50]  # use reasonable ticks
plt.yticks(yticks, yticks)

## Symlog limits (log-linear-log)
plt.ylim((-1000.,1000.)); plt.yscale('symlog', linthreshy=10.); 

## Set limits individually
plt.xlim(left=0,    right=0)
plt.ylim(bottom=0,  top=0)


## Sophisticated ticks on the x-axis
xlim = (-1, 41)
xunit = 1.
def reasonable_ticks(a): 
    """ Define the grid and ticks a bit denser than by default """
    x=np.trunc(np.log10(a)); y=a/10**x/10
    return (10**x, 2*10**x,5*10**x)[np.int(3*y)]
xticks = np.arange(xlim[0], xlim[1], reasonable_ticks((xlim[1]-xlim[0])/10))
xnumbers = [("%.2f"%(f/xunit) if abs(f%reasonable_ticks((xlim[1]-xlim[0])/5))<(xunit/1000) else "") for f in xticks]
plt.xticks(xticks, xnumbers); plt.minorticks_on();  plt.grid(True)


## ==== Drawing graphics ====
## Vertical span
plt.axvspan(from_x, to_x, color='r')

## Pretty annotate (with text and arrow)
## `xycoords' and `textcoords' can be chosen from  {offset|figure|axes}:{points|pixels|fraction} | data
bboxprops   = dict(boxstyle='round, pad=.15', fc='white', alpha=1.)
arrowprops  = dict(arrowstyle=('->', '-|>', 'simple', 'fancy')[0], connectionstyle = 'arc3,rad=0', lw=1, ec='m', fc='w')
plt.annotate('text',                    # set empty to disable text
        xy      = (.4,  .4),    xycoords  ='figure fraction',
        xytext  = (.3, .3),    textcoords='figure fraction',        # (delete this if text without arrow is used)
        ha='right', va='bottom', size=25, color='b',
        bbox        = bboxprops,        # comment out to disable bounding box
        arrowprops  = arrowprops,       # comment out to disable arrow
        )


## ==== Basic operations  ====
#complex to polar
#polar to complex
#Find maxima
#Find zero passes for function stored as `x', `y' arrays
from scipy.optimize import fsolve
estimates = x[np.where(np.diff(np.sign(y)))[0]]
passes_at_x = fsolve(lambda x0: np.interp(x0, x, y),  estimates)

# Interpolate (.. resample to points in `xr')
xr = np.linspace(0., 3.5, 300)
yr = np.interp(xr, x, y)

# Interpolate 2D data?

## Least squares fit of function
def func(x0, ampli, center, width):                      # define the function to be used for fitting
    """ Gaussian function """
    return ampli * np.exp(-((x0 - center)/width)**2)
from scipy.optimize import curve_fit
popt, pcov = curve_fit(func, x, y)
try:                                                    # (optional) print(the fit parameters for the user)
    print("Fit parameters:", zip(func.func_code.co_varnames[1:], popt)  # give parameter name and value (if possible))
except:
    print("Fit parameters:", popt)

## Optimize polynomial fit

## ==== Transforms ====
## Integrate.trapz
## Derivative
## Derivative2



## ==== Matrices and linear algebra ====
dot
import linalg as la
## Plot a function of complex argument FIXME duplicates
xr = np.linspace(-5,5,100)
yr = np.linspace(-5,5,100)
xm, ym = np.meshgrid(xr,yr)
plt.contourf(xr,yr, map(lambda p: np.arccos(p).imag, xm+1j*ym), 100) 
plt.colorbar(), plt.show()



## ==== Fourier transform ====
## 1D FFT with cropping for useful frequencies
freq    = np.fft.fftfreq(len(x), d=(x[1]-x[0]))         # calculate the frequency axis with proper spacing
yf      = np.fft.fft(y, axis=0) / len(x) * 2*np.pi      # calculate the FFT values
freq    = np.fft.fftshift(freq)                         # ensures the frequency axis is a growing function
yf      = np.fft.fftshift(yf) / np.exp(1j*2*np.pi*freq * x[0])   # dtto, and corrects the phase for the case when x[0] != 0
truncated = np.logical_and(freq>0, freq<np.inf)         # (optional) get the frequency range
(yf, freq) = map(lambda array: array[truncated], (yf, freq))    # (optional) truncate the data points
plt.plot(freq, np.abs(yf), color="#FF8800", label=u"$y'$", ls='-')                  # (optional) plot amplitude
plt.plot(freq, np.unwrap(np.angle(yf)), color="#FF8800", label=u"$y'$", ls='--')    # (optional) plot phase

## 2D FFT Fourier transform using scipy
import scipy.fftpack
zf = scipy.fftpack.fft2(z)
zf = scipy.fftpack.fftshift(zf)

## Optionally pad the data range to power of two for better FFT performance
target_len = 2**np.ceil(np.log(len(y)*1)/np.log(2))
append_len = target_len - len(y)


## ==== Data analysis ====

## Least squares fit of function
def func(x0, ampli, center, width):                      # define the function to be used for fitting
    """ Gaussian function """
    return ampli * np.exp(-((x0 - center)/width)**2)
from scipy.optimize import curve_fit
popt, pcov = curve_fit(func, freq, np.abs(yf))
try:                                                    # (optional) print(the fit parameters for the user)
    print("Fit parameters:", zip(func.func_code.co_varnames[1:], popt)  # try to link parameter names to values)
except:
    print("Fit parameters:", popt)



## Inverse Fourier transform
## Hilbert transform
## Laplace??


## 2D FFT Fourier transform [from (t,Et) to (kT, Ef)], with FFTshift on both axes
#  # (along the time axis `t')
#  freq    = np.fft.fftfreq(len(t), d=(t[1]-t[0]))         # calculate the frequency axis with proper spacing
#  Efy     = np.fft.fft(Et, axis=1) / len(t) * 2*np.pi     # calculate the FFT values
#  freq    = np.fft.fftshift(freq)
#  Efy     = np.fft.fftshift(Efy)
#  ## (along the spatial axis `y')
#  kT      = np.fft.fftfreq(len(y), d=(y[1]-y[0]))        # calculate the frequency axis with proper spacing
#  Ef      = np.fft.fft(Efy, axis=0) / len(y) * 2*np.pi     # calculate the FFT values
#  kT      = np.fft.fftshift(kT)
#  def fftshift2(arr): return np.vstack([arr[len(arr)/2:,:], arr[:len(arr)/2,:]]) ## (workaround)
#  Ef =  fftshift2(Ef)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
