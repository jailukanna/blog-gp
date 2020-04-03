---
title: matplotlib example voronoi subdivide (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'voronoi subdivide'


Modules used in program: 
* `import matplotlib.tri as tri`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`

## python voronoi subdivide

Python matplotlib example: voronoi subdivide

```python
import numpy as np
from scipy.spatial.distance import cdist,pdist,squareform
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.tri as tri

from voronoi import voronoi # http://webloria.loria.fr/~rougier/coding/neural-networks/voronoi.py


# Code to take a voronoi tesselation of a 2D space and subdivide each voronoi cell by 
# selecting the m furthest neighboring voronoi cell centers and generating n-1 new points between
# the center and the midpoint to the neighbor, that along with the original center will 
# define a new voronoi parition within the cell.


k = 10 # number of reference poses
N = 40000 # number of test points
m = 2 # number of neighbors to base subdivision off of
n = 3 # number of slices 

colors = np.arange(m*n+1)

g = np.linspace(0,1,n+1)
gdata = np.array([0,1])

# Define Random reference points
x_i = np.random.uniform(0,5,size=(k,2))

# Calculate pairwise distance between references points
d_ij = squareform(pdist(x_i))

# Define voronoi
vorbounds = voronoi(x_i[:,0],x_i[:,1])

# Create Deluanay Triangulation
triang = tri.Triangulation(x_i[:,0], x_i[:,1])
edges = triang.edges

# Create supplementary poses to subdivide voronoi cells
y_i = []
for vori,vor in enumerate(x_i):
    temppoints = []
    # Identify neighboring reference poses
    jj = np.where((edges[:,0] == vori) | (edges[:,1] == vori))
    pj = edges[jj].flatten()
    pj = pj[pj != vori]
    
    # Get m poses with largest distance
    d_j = d_ij[vori,:]
    neigh_target_j = pj[np.argsort(d_j[pj])[::-1]]
    
    # Linearly interpolate n points between reference and midpoint with neighbors
    count = 0
    for nj in neigh_target_j:
        if count > m - 1:
            break
        midpoint = 0.5*(vor - x_i[nj,:]) + x_i[nj,:]
        nearest = np.argmin(cdist(np.atleast_2d(midpoint),x_i))
        # exclude centeres connected via the Delaunay triangulation
        # that pass through an intervening voronoi cell
        if not nearest in [nj,vori]:
            continue
        endpoints = np.vstack((vor,midpoint))
        a = np.zeros((g.shape[0]-2,2))
        for dim in xrange(2):
            a[:,dim] = np.interp(g,gdata,endpoints[:,dim])[1:-1]
        
        temppoints.append(a)
        count += 1
    
    if len(temppoints):  
        y_i.append(np.vstack(temppoints))
    else:
        y_i.append([])

# Generate a large number of random configs and assign to subdivisions
x = np.random.uniform(0,5,size=(N,2))
cassign = np.zeros((N,))
for ii,p in enumerate(x):
    # Calculate the distance from point to every reference pose
    p = np.atleast_2d(p)
    d_j = cdist(p,x_i,metric='euclidean')
    
    # Identify voronoi membership
    pi = np.argmin(d_j)
    
    # Determine membership in subdivisions
    if len(y_i[pi]):
        subpoints = np.vstack((x_i[pi,:],y_i[pi]))
    else:
        subpoints = x_i[pi,:]
    subpoints = np.atleast_2d(subpoints)
    si = np.argmin(cdist(p,subpoints))
    cassign[ii] = colors[si]
    
#print(cassign    )
fig = plt.figure()
ax = fig.add_subplot(111)

# Plot reference poses
ax.plot(x_i[:,0],x_i[:,1],'.',color='yellow',markersize=20)

# Plot internal points
y_ii = [a for a in y_i if a != []]
z = np.vstack(y_ii)
ax.plot(z[:,0],z[:,1],'.',color='yellow',markersize=10)


# Plot voronoi edges
vorlines = matplotlib.collections.LineCollection(vorbounds, color='yellow',linewidths=3)
ax.add_collection(vorlines)

# Plot Deluanay triangulation
ax.triplot(triang, 'wo-')

# Plot test particles
ax.scatter(x[:,0],x[:,1],marker='o',c=cassign,s=5)

plt.axis([-1,6,-1,6])
plt.show()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
