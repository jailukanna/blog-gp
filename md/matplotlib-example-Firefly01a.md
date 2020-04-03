---
title: matplotlib example Firefly01a (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Firefly01a'

Functions in program: 
* `def update(i):`
* `def foo(A,i,j,r):`

Modules used in program: 
* `import matplotlib.animation as anim`
* `import matplotlib.axes as axes`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pylab as pyl`
* `import matplotlib`
* `import numpy as np`
* `import random`

## python Firefly01a

Python matplotlib example: Firefly01a

```python
####### PROJECT : FIREFLIES SYNCHRONIZATION 

####### MODEL SYNCHRONISED TO THE FASTEST ONE
####### RADIUS OF INTERACTION : 1-2-3-...
####### POSITION STEADY

#######  LIBRARIES  ###########

import random
import numpy as np
import matplotlib
import matplotlib.pylab as pyl
import matplotlib.pyplot as plt
import matplotlib.axes as axes
import matplotlib.animation as anim

####### FUNCTIONS

####### FUNCTION OF THE NEIGHBORHOOD DEPENDING ON RADIUS
def foo(A,i,j,r):
    return A[max(i-r,0):max(i+r+1,0),max(j-r,0):max(j+r+1,0)]

####### FUNCTION FOR DYNAMICAL PLOT
def update(i):
    return [plt.imshow(Z[i],cmap='plasma',vmin=0,vmax=1)] 

#######  DEFAULTS   ##############

N=int(input('Dwse diastasi plythismou pygolampidwn NXN : ')) # N DIMENSION SIZE OF THE FIREFLIES MATRIX : N x N
Rmin,Rmax = 100,300  # RANGE OF FIRES

neighbor=int(input('Dwste emveleia radar 1,2,3 : '))

######  RANGE MATRIX

x=[]
for i in range(N):
    x_temp=[]
    for j in range(N):
        x_temp.append(np.random.randint(Rmin,Rmax))
    x.append(x_temp)

x=np.array(x)

###### SYNCHRONIZATION
count=0
SUM=[]

while True :
    count+=1
    m=[]
    for i in range(N):
        m_temp=[]
        for j in range(N):
            temp=foo(x,i,j,neighbor).min()  # neighborhood matrix 
            m_temp.append(temp)             # minimum range in the nighborhood
        m.append(m_temp)

    m=np.array(m)

    maxm=m.max()
    print(count,m.max()-m.min())

    
###### THE BINARY SIGNAL

    S=N*[N*[(maxm+1)*[0]]] # 3d array
    S=np.array(S)

    for i in range(N):
        for j in range(N):
            S[i][j][m[i,j]]=1

##### THE FINAL 3D ARRAY OF BINARY ON OFF LIGHT AND TIME

    if count>1 :    # if SUM already exist
        SUM.append(S.T)
    else :          # create SUM
        SUM.append(S.T)

###### CHECKING FOR THE STATE OF SYNCHRONIZATION

    if (m==m[0]).all() :
        print('Epilthe sygxronismos se '+str(count)+' vimata')
        break

    x=m

###### VISUALIZATION OF THE PHAINOMENO

imcount=0

Z=[]
im=[]

for p in range(len(SUM)):
    SUM[p].tolist()
    for t in range(len(SUM[p])):
        Z.append(SUM[p][t].tolist())
        imcount+=1

Z=np.array(Z)

j=0

fig=plt.figure('Pygolampides')

ani=anim.FuncAnimation(fig,update,frames=np.array(list(range(imcount))),interval=1,blit=True,repeat=False)
    
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
