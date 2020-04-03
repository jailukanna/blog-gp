---
title: matplotlib example sed (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sed'

Functions in program: 
* `def logblack(*args):`
* `def black(v, beta, T, logA):`
* `def thermaldust(nu, beam, T_d, tau, beta):`
* `def tick_function(x):`

Modules used in program: 
* `import matplotlib.dates as mdates`
* `import datetime`
* `import sys`
* `import matplotlib.pyplot as plt`
* `import scipy `
* `import matplotlib`
* `import numpy as np`

## python sed

Python matplotlib example: sed

```python
import numpy as np
import matplotlib
import scipy 
from scipy.optimize import curve_fit
#matplotlib,pgetu,emcee,astropy,warnings
#from scipy import ndimage,optimize; from scipy.optimize import fmin
import matplotlib.pyplot as plt
import sys
import datetime
import matplotlib.dates as mdates
from matplotlib.transforms import Transform
from matplotlib.ticker import (
    AutoLocator, AutoMinorLocator)

plt.rcParams['mathtext.default']='regular'

"""
plot a given expression y=f(x) 

x= np.linspace(1,5)
y = np.power(x,3)
plt.plot(x,y,color='blue')
plt.plot(np.log(x),np.log(y),color='green')
plt.show()

#############################

x= np.linspace(1,5)
y = np.power(x,3)
#plt.plot(x,y,color='blue')
plt.loglog(x,y,color='green')
plt.show()

"""
"""

x=[250/2.524,350/2.524,500/2.524,59958.4916/2.524]
y=[67e-3,56e-3,32e-3,15e-6]

plt.loglog(x,y,'go')
plt.xlim(10,1e5)
plt.ylim(1e-6,1)
#plt.show()
plt.savefig('hrs.png')

"""



"""
c = 3.0e14 #micro-m/s
#1+z = 2.524

x=[250/2.524,350/2.524,500/2.524,1313.53/2.524,59958.4916/2.524,54507.72/2.524]    #54507.72 micro-m = 5.5 GHz = our VLA frequeny, 90 micro-Jy is the total flux density
y=[67e-3,56e-3,32e-3,21.99e-3,15e-6,90e-6]      #228.234 GHz =1313.53 micro-m ; 21.99 mJy Divide by 1+z to get the rest wavelength

fig = plt.figure()
ax1 = fig.add_subplot(111)
ax2 = ax1.twiny()

#fig, ax = plt.subplots()
ax1.loglog(x, y,'go')
plt.xlim(10,1e5)
ax1.set_xlabel('Rest Wavelength ($\mu$m)')
ax1.set_ylabel('Flux Density (Jy)')

new_tick_locations = np.array([1e2,1e3,1e4,1e5])

def tick_function(x):
    V = c/(2.524*x)
    return ["%.4f" % z for z in V]

ax2.set_xlim(ax1.get_xlim())
ax2.set_xticks(new_tick_locations)
ax2.set_xticklabels(tick_function(new_tick_locations))
ax2.set_xlabel(r"Modified x-axis: $1/(1+X)$")
plt.show()


"""
"""

# Thermal Dust Emission

def thermaldust(nu, beam, T_d, tau, beta):

    c = 299792458.
    k = 1.3806488e-23
    h = 6.62606957e-34
    nu = np.multiply(nu,1e9)

    planck = np.exp(h*nu/k/T_d) - 1.
    modify = 10**tau * (nu/353e9)**beta # set to tau_353
    S = 2 * h * nu**3/c**2 /planck * modify * beam * 1e26

    return S

"""

h = 6.63e-34 #m2 kg / s
k = 1.38e-23 #m2 kg s-2 K-1
c = 3.0e14 #micro-m/s


restwave=[250/2.524,350/2.524,500/2.524,1275.71/2.524 ,59958.4916/2.524,54507.72/2.524]    #54507.72 micro-m = 5.5 GHz = our VLA frequeny, 90 micro-Jy is the total flux density
flux=[67e-3,56e-3,32e-3,2.19e-3,15e-6,90e-6]      #228.234 GHz =1313.53 micro-m ; 21.99 mJy Divide by 1+z to get the rest wavelength
# 235 GHz = 1275.71 micro-m, flux = 2.19 mJy


def black(v, beta, T, logA):
     vhkt = np.array(v) * (h/(k*T))
     planck = np.exp(vhkt) - 1
     vpow = np.exp( np.log(v) * (3+beta))
     S = np.power(10.,logA)*np.divide(vpow, planck)
     return S
    
p0 = [1.5,31,-56]
#p0 =[1.5, 31, -20]
#p0 = [ -3.4163290769976857, 34.90730748112709, -10.190671841431119 ]
#p0 = [ -3.44728436, 68.64392877, -10.4031822 ]
#p0 = [ -3.44728436, 68.64392877, -10.4031822 ]


# Do initial sanity check
print('Input | Output')
for inp, oup in zip(restwave, flux):
    print(black(inp, *p0), ', ', oup)

# Fit with square of log loss first to get balllpark parameters
def logblack(*args):
    return np.log(black(*args))
initial_params, _ = curve_fit(logblack, restwave, np.log(flux), p0=p0)

print('Initial fitted params are [', ', '.join([str(x) for x in initial_params]), ']')

# Fit with updated initial params
params, cov = curve_fit(black, restwave, flux, p0=initial_params)#, sigma=[1e-3,1e-3,1e-3,1e-3])# SIGMA parameter is optional. 
#params_err = np.sqrt(np.diag(cov))

print('Final fitted params are [', ', '.join([ str(x) for x in params]), ']')


### convert rest wave to rest freq, because freq is needed in the BB equation ###
restfreq = []
restfreq= [c*(i**-1) for i in restwave]

# Plot curve
modelxrangewave = np.linspace(np.min(restwave), np.max(restwave), 1000)
modelxrangefreq = [c*(i**-1) for i in modelxrangewave]
fitted_model = black(modelxrangewave, *params)
plt.loglog(modelxrangewave,fitted_model)

# Plot data
plt.loglog(restwave, flux,'go')

# Plot predictions of fitted model
plt.loglog(restwave, black(restwave, *params),'ro')

plt.xlim(10,1e5)
#plt.ylim(1e-6,1)
plt.xlabel('Rest Wavelength ($\mu$m)')
plt.ylabel('Flux Density (Jy)')
plt.show()
#plt.savefig('updated3.png')






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
