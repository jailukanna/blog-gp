---
title: matplotlib example classR (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'classR'


Modules used in program: 
* `import pickle`
* `import urllib.request`
* `import pickle`
* `import rpy2.robjects.numpy2ri as npr   `
* `import rpy2.robjects.packages as rpackages`
* `import rpy2.robjects as robjects`
* `import rpy2`
* `import numpy as np`

## python classR

Python matplotlib example: classR

```python
# helper class to interface between R and python using rpy2
# &copy; Monte J. Shaffer
# monte.shaffer@gmail.com
# MIT license
import numpy as np

import rpy2
import rpy2.robjects as robjects
import rpy2.robjects.packages as rpackages
from rpy2.robjects.packages import importr

import rpy2.robjects.numpy2ri as npr   
npr.activate()

base = importr('base')
utils = importr('utils')
stats = importr('stats')
# helper class to interface between R and python using rpy2
class classR:

    def __init__(self, plotme=True):
        self.version = {}
        self.version["rpy2"] = rpy2.__version__
        from rpy2.rinterface import R_VERSION_BUILD
        self.version["R"] = R_VERSION_BUILD
        system = base.Sys_info() # base.set_seed(123)
        #utils.str(system)
        self.version["bit"] = system.rx2("machine")
        print(system)
        
        # rather than doing a bunch of 'from ... import'
        # common functions here
        # http://rpy.sourceforge.net/rpy2/doc-2.2/html/vector.html#creating-vectors
        self.Vector         = robjects.Vector
        
        self.IntVector      = robjects.IntVector
        self.FloatVector    = robjects.FloatVector
        self.FactorVector   = robjects.FactorVector
        self.BoolVector     = robjects.BoolVector
        self.StrVector      = robjects.StrVector
        self.ListVector     = robjects.ListVector
        
        self.plotme = plotme
        if(plotme == True): 
            # http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyqt4
            # pip install .\PyQt...
            # https://github.com/matplotlib/matplotlib/issues/2134/
            import matplotlib
            matplotlib.use("qt4agg")
            from matplotlib import pyplot as plt
            self.plt = plt            
            print("pyplot loaded")
        
        
        
    def mirror(self,idx=1):
        utils.chooseCRANmirror(False,ind=idx) # F and FALSE don't work, False and 0 works
        
    def library(self,package):
        return importr(package)
        
    def install(self,package):
        utils.install_packages(package)
        
    def print(self,what):
        base.print(what)
        
    def summary(self,what):
        base.summary(what)
        
    def str(self,what):
        utils.str(what)
     
    def castValue(self,val):
        vlen = len(val)
        if vlen == 1:
            return val[0]
        else:
            return np.asarray(val)
        return val
    
    def getValue(self,what):    # may not work on dataframe element rx not rx2
        a = what.split("$")     # partition
        e = str(a[0])
        for i in range(1,len(a)):
            e = e + ".rx2(" + '"' + str(a[i]) + '"' + ")"
        val = eval(e)
        return self.castValue(val)
        


# http://ipython.readthedocs.io/en/stable/interactive/tutorial.html
# POWERSHELL ... cd C:\Python3.6.3\_monte_\
# ipython
# %run -d classR.py 


#####################################################################
#  USAGE
#####################################################################

""" 
# web delivery of data 'points'
import pickle
import urllib.request
raw = urllib.request.urlopen('http://www.mshaffer.com/arizona/dissertation/points').read()
points = pickle.loads(raw) # notice plural
"""


"""
# local delivery of data 'points'
import pickle
with open ('points', 'rb') as fp:
    points = pickle.load(fp) 
    
# initiate class  
R = classR()

# R.mirror(1)       # choose mirror [1 - Cloud] before installing package
# R.install("fpc")
fpc         = R.library("fpc")
cluster     = R.library('cluster')


k = R.IntVector(range(3, 8)) # r-syntax  3:7   # I expect 5

pamk_clusters = fpc.pamk(points,k)       # %time  # very slow    [Wall time: 1min 11s]

# k = 5 ... as expected

pam_clusters = cluster.pam(points,5)     # %time  # much slower  [Wall time: 10.6 s]
kmeans_clusters = stats.kmeans(points,5) # %time  # much faster  [Wall time: 5.5 ms]

print(kmeans_clusters)
R.print(kmeans_clusters)
R.str(kmeans_clusters)

centers = R.getValue("kmeans_clusters$centers")  
wss = R.getValue("kmeans_clusters$withinss")
tot = R.getValue("kmeans_clusters$tot.withinss")

wss/tot

size = R.getValue("kmeans_clusters$size")

member = R.getValue("kmeans_clusters$cluster")  # cluster may be package

nmember = member.reshape(len(member),1)

npoints = np.hstack((points,nmember))

plt.figure("scatter")
plt.scatter(npoints[:,0],npoints[:,1],c=npoints[:,2])
plt.show(block=False)

sidx = np.argmax(size)
smax = size[sidx]
smember = 1 + sidx
scenter = centers[sidx]

spoints = npoints[npoints[:,2]==smember]

minx = min(spoints[:,0])
maxx = max(spoints[:,0])
cx = (minx + maxx)/2

miny = min(spoints[:,1])
maxy = max(spoints[:,1])
cy = (miny + maxy)/2

deltax = cx - scenter[0]
deltay = cy - scenter[1]

w = maxx - minx
h = maxy - miny

from matplotlib.patches import Ellipse

plt.figure("scatter")
plt.scatter(spoints[:,0],spoints[:,1],c=spoints[:,2])
plt.scatter(scenter[0],scenter[1])

ax = plt.gca()
ellipse = Ellipse(xy=(scenter[0] + deltax,scenter[1] + deltay), width=w, height=h, 
                        edgecolor='r', fc='None', lw=2)
ax.add_patch(ellipse)
plt.show(block=False)
"""



  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
