---
title: matplotlib example despottic example coSLED (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'despottic example coSLED'

Functions in program: 
* `def ranges(min,max):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python despottic example coSLED

Python matplotlib example: despottic example coSLED

```python
########################################################################
# 
# sled_test1.py
#
# This is an example code using the DESPOTIC library. This code will 
# compute the SLEDs for CO12 and CO13 on several cloud files which
# will be progressing from the Milky Way temperature and density to the
# Ulirg temperature and density. Also calculates all of the X_CO factors
# for the clouds in question. See despotic example code from install dir
#
########################################################################

########################################################################
# Library Imports
########################################################################

# Import the despotic library
from despotic import cloud
import numpy as np
# Import standard python libraries
#from numpy import *
#from matplotlib import *
#from matplotlib.pyplot import *
from datetime import datetime
from datetime import timedelta


#######################################################################
# Import simulated cloud file
#######################################################################
# Read the Milky Way GMC cloud file
gmc = cloud(fileName='cloudfiles/MilkyWayGMC.desp')

# Read the ULIRG cloud file
ulirg = cloud(fileName='cloudfiles/ULIRG.desp')

#######################################################################
# The calculations
#######################################################################

"""
Change the densities of the cloud and move closer to the ULIRG T and 
rho...
"""
def ranges(min,max):
    """ 
    Return a list with boundaries at Milky Way and ULIRG conditions
    and three intermediate points.
    """
    return [min+i*(max-min) for i in [0.00,0.25,0.50,0.75,1.00]]

maxDens = 1.0e5 # ULIRG volume gas density
minDens = 100.0 # Milky Way volume gas density
rangesDens = ranges(minDens,maxDens)

maxT = 45 # Kelvin temp of the ULIRG cloud
minT = 8 # Kelvin temp of the Milky Way cloud
rangesT = ranges(minT,maxT)

# Compute the luminosity of the CO lines in the cloud at each 
luminosities = [] # array to store the luminosities
luminosities13 = [] # foc CO13
XCO = [] # XCO's
gmc = cloud(fileName='cloudfiles/MilkyWayGMC.desp')
for i in range(5):
    ##gmc = cloud(fileName='cloudfile/MilkyWayGMC.desp')
    # change the Temperature and density
    gmc.nH = rangesDens[i]
    gmc.Tg = rangesT[i]
    # compute the luminosity of the CO lines in the cloud
    t1 = datetime.now()
    gmclines = gmc.lineLum('co')
    gmclines13 = gmc.lineLum('13co')
    t2 = datetime.now()
    luminosities.append(gmclines)
    luminosities13.append(gmclines13)
    print("Execution time = "+str(t2-t1))
    # Print out the CO X factor for both clouds. This is column density           
    # divided by velocity-integrated brightness temperature.
    print("GMC X_CO = "+str(gmc.colDen/gmclines[0]['intTB']) + \)
        " cm^-2 / (K km s^-1)" + "At gas dens ",gmc.nH," and temperature ",\
        gmc.Tg
    XCO.append(gmc.colDen/gmclines[0]['intTB'])

import matplotlib.pyplot as plt
plt.plot(XCO)
plt.savefig("jnk.png")

# Some other code from M. Krumholz
gmcTBint=np.array([line['intTB'] for line in gmclines])
gmcTBint13=np.array([line['intTB'] for line in gmclines13])

# Mask negative values to avoid warnings. Negative values are obtained
# for some of the very high J states due simply to roundoff error.
gmcTBint[gmcTBint <= 0] = 1e-50
gmcTBint13[gmcTBint13 <= 0] = 1e-50

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
