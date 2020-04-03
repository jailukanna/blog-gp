---
title: matplotlib example readdosedata (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'readdosedata'

Functions in program: 
* `def plotProfile(dosedata,depth):`
* `def plotOARy(dosedata):`
* `def plotOARx(dosedata):`
* `def plotPDD(dosedata):`
* `def readdosedata(filename):`

Modules used in program: 
* `import sys,getopt`
* `import matplotlib.pyplot as plt`
* `import matplotlib.mlab as mlab`
* `import matplotlib.cm as cm`
* `import numpy as np`
* `import matplotlib`

## python readdosedata

Python matplotlib example: readdosedata

```python
#!/usr/bin/python

import matplotlib
import numpy as np
import matplotlib.cm as cm
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt
import sys,getopt

#class dosedata:
#	def __init__(self,):

def readdosedata(filename):
	datafile = open(filename)
	N = np.fromstring(datafile.readline(),dtype=int, sep = ' ')
	N[0],N[2] = N[2],N[0]
	bx = np.fromstring(datafile.readline(),dtype=np.double, sep = ' ')
	by = np.fromstring(datafile.readline(),dtype=np.double, sep = ' ')
	bz = np.fromstring(datafile.readline(),dtype=np.double, sep = ' ')
	dose = np.fromstring(datafile.readline(),dtype=np.double, sep = ' ')
	dose.shape = N
	doseerr = np.fromstring(datafile.readline(),dtype=np.double, sep = ' ')
	doseerr.shape = N
	datafile.close()
	return [N,bx,by,bz,dose,doseerr]

def plotPDD(dosedata):
	dose=dosedata[4]
	bz = dosedata[3]
	N=dosedata[0]
	plt.plot(bz[:-1],dose[:,N[1] /2,N[2] /2]/((dose[7,N[1] /2,N[2] /2]+dose[8,N[1] /2,N[2] /2])/2))
	plt.xlabel("depth(cm)")
	#plt.ylabel("dose(Gy/particle)")
	plt.ylabel("nomalized dose")
	plt.title('PDD of 60mm Collimator')
	locs = plt.yticks()[0]
	labels = ["%g" % l for l in locs]
	plt.yticks(locs,labels)
	plt.show()

def plotOARx(dosedata):
	dose=dosedata[4]
	bx = dosedata[1]
	N=dosedata[0]
	plt.plot(bx[:-1],dose[N[0] /2,N[1] /2,:]) #/((dose[7,N[1] /2,N[2] /2]+dose[8,N[1] /2,N[2] /2])/2))
	plt.xlabel("x(cm)")
	#plt.ylabel("dose(Gy/particle)")
	plt.ylabel("nomalized dose")
	plt.title('OAR in x direction of 60mm Collimator')
	locs = plt.yticks()[0]
	#labels = ["%g" % l for l in locs]
	#plt.yticks(locs,labels)
	plt.show()

def plotOARy(dosedata):
	dose=dosedata[4]
	by = dosedata[2]
	N=dosedata[0]
	plt.plot(by[50:72],dose[N[0] /2,50:72,N[2] /2])#/((dose[7,N[1] /2,N[2] /2]+dose[8,N[1] /2,N[2] /2])/2))
	plt.xlabel("y(cm)")
	#plt.ylabel("dose(Gy/particle)")
	plt.ylabel("nomalized dose")
	plt.title('OAR in y direction of 60mm Collimator')
	#locs = plt.yticks()[0]
	#labels = ["%g" % l for l in locs]
	#plt.yticks(locs,labels)
	plt.show()

def plotProfile(dosedata,depth):
	matplotlib.rcParams['xtick.direction'] = 'out'
	matplotlib.rcParams['ytick.direction'] = 'out'
	X = dosedata[1][0:-1]
	Y = dosedata[2][0:-1]
	Z = dosedata[3]
	depthIndex = 0
	while Z[depthIndex] < depth:
		depthIndex += 1
	plt.figure()
	CS = plt.contourf(X,Y,dosedata[4][depthIndex,:,:])
	# plt.clabel(CS, inline = 1,fontsize = 10)
	plt.show()

if __name__ == '__main__':
	opts,args = getopt.getopt(sys.argv[1:],"xyz","")
	dosedata = readdosedata(args[0])
	opt = opts[0]
	if opt[0] == '-z':
		plotPDD(dosedata)
	if opt[0] == '-x':
		plotOARx(dosedata)
	if opt[0] == '-y':
		plotOARy(dosedata)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
