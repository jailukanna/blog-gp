---
title: matplotlib example QMtunnelling (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'QMtunnelling'

Functions in program: 
* `def V2(x):`
* `def V(x):`

Modules used in program: 
* `import matplotlib.pyplot`
* `import numpy as np`

## python QMtunnelling

Python matplotlib example: QMtunnelling

```python
from __future__  import division
import numpy as np
import matplotlib.pyplot

dt = .001 #delta t
ts = 5000 #time steps each row is eq. to dt/2 
dx = 0.1 #Bohr Radius
ls = 350 #length step
length = ls * dx # Bohr Radii


m = 1 # mass of electron
w = 2 # frequency of wave
k = 6 # wave vector magnitude
sigma = .5 # standard deviation; meh.
xnot = length / 2 # initial position of wave packet
hbar = 1 #momentum unit

## POTENTIAL BARRIER STUFF ##
VL = 0
VM = 25
VR = 0
POS1 = 3 * length / 5
POS2 = 4 * length / 5

#~ print('length steps:' , ls)

impsi = np.zeros((ts,ls)) #imaginary component of the psi
repsi = np.zeros((ts,ls)) #real component of the psi

# set initial conditions for a wavepacket
for j in range(ls):
	x = j*dx;
	impsi[0,j] = np.exp((-(x-xnot)**2)/(2*sigma**2))*np.sin(k*(x)-w*(-dt/2))
	repsi[0,j] = np.exp((-(x-xnot)**2)/(2*sigma**2))*np.cos(k*(x)-w*(0))

#~ print('\n impsi: \n' , impsi)
#~ print('\n repsi: \n' , repsi)

def V(x):
	# if (x > POS1) and (x < POS2):
		# return VM
	if x > POS2:
		return VR
	elif x < POS1:
		return VL
	else:
		return VM

def V2(x):
	return 0.5 * k * (x-(length / 2))**2

# populate remaining time steps
for i in range(1,ts):
	for j in range(1, ls-1):
		impsi[i,j] = impsi[i-1,j] + ((dt * (hbar / (2 * m)) * (repsi[i-1,j+1] + repsi[i-1,j-1] - (2 * repsi[i-1,j]))) / (dx**2))  - (dt * V(j*dx) * repsi[i-1,j] / hbar)
	
	for j in range(1, ls-1):
		repsi[i,j] = repsi[i-1,j] - (dt * (hbar / (2 * m)) * (impsi[i,j+1] + impsi[i,j-1] - (2 * impsi[i,j]))) / (dx**2) + (dt * V(j*dx) * impsi[i,j] / hbar)

#~ print('\n impsi: \n' , impsi)
#~ print('\n repsi: \n' , repsi)

#~ matplotlib.pyplot.ion() # allows for interaction
XV = np.arange(0,ls)
XLabels = np.arange(0, ls)
XLabels = XLabels * dx

print(XLabels)

# Generate an array to plot V2(x) #
for j in range(ls):
	XV[j] = V2(j*dx)

while (1):
	for i in range(1, ts-1):
		if i%10==0:
			matplotlib.pyplot.cla() # clears current plot
			matplotlib.pyplot.plot(XLabels, repsi[i,:])
			matplotlib.pyplot.plot(XLabels, impsi[i,:], 'r')
			matplotlib.pyplot.xlim(0, XLabels[ls-1])
			matplotlib.pyplot.ylim(-1,1)
			matplotlib.pyplot.xlabel("Position")
			matplotlib.pyplot.ylabel("Amplitude")
			#~ matplotlib.pyplot.axvline(POS1)
			#~ matplotlib.pyplot.axvline(POS2)
			#~ #matplotlib.pyplot.ylim(-0.2,0.2)
			matplotlib.pyplot.pause(1e-9) # redraws canvas 10*10**(-3)

	matplotlib.pyplot.show()

# you were working on doing this to induce numerical stability in your dt for part 3 of the weekly goals, everythin else is currently working

#dt = .5 /((hbar**2/(2*m))*(1/(dx**2))) #delta t #delta t


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
