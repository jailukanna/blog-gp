---
title: matplotlib example cheatsheet (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'cheatsheet'


Modules used in program: 
* `import pandas`
* `import numpy as np`
* `import IPython.display`
* `import numpy`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import datetime`

## python cheatsheet

Python matplotlib example: cheatsheet

```python
#pyhton cheatsheet

#######################################
########## basic syntax ###############
#######################################

#How to sort a dict by value :
for w in sorted(counter, key=counter.get, reverse=True):
    pass
    


 
    
sentences = open('book.txt', encoding="utf-8").read().split(".")

#time
import datetime

start_time = datetime.datetime.now()

print('Time elapsed:', datetime.datetime.now() - start_time)

#generator
a = [ x for x in range( 0 , N ) if f(x) ]

#class def
class MyClass:
    def __init__( self , args ):
        pass
        
        
from A import B

for x in X:
    pass

for i,x in enumarate(X):
    pass

a = zip(range(0,10),range(10,20))

#######################################
############## matplotlib #############
#######################################

#boilerplate
from matplotlib.pyplot import imshow
import matplotlib.pyplot as plt
from PIL import Image
%matplotlib notebook

#draw an 2d array as heatmap
src = np.ndarray([N,N])
imshow(np.asarray(src))

#draw a 3d grid
src = np.ndarray([N,N])
hf = plt.figure()
ha = hf.add_subplot(111, projection='3d')
X = np.arange(0, N)
Y = np.arange(0, N)
X, Y = np.meshgrid(X, Y)
ha.plot_surface(X, Y, src)
plt.tight_layout()
plt.show()

#draw a plot
Y = np.ndarray( [N,1])
hf = plt.figure()
plt.plot(Y)
plt.show()

#point cloud
import matplotlib.pyplot as plt
alpha = plt.figure()
x, y = np.ndarray([N]), np.ndarray([N])
plt.scatter(x, y,  s=1)
plt.show()

#######################################
############## Image ##################
#######################################

from PIL import Image
import numpy
raw_image = Image.open( "test.png" )
image = numpy.asarray( raw_image.convert( 'L' ) ).astype( numpy.float32 ).ravel() / 255.0
print( image.shape )

result = Image.fromarray( ( numpy.reshape( result_image , ( raw_image.size[ 0 ] , raw_image.size[ 1 ] ) ) * 255 ).astype( numpy.uint8 ) )
result.save( "result.png" )

import IPython.display
IPython.display.Image( filename = "result.png" ) 

#######################################
############## NumPy ##################
#######################################

import numpy as np
x = numpy.random.random( ( N , M ) ) * 2 - 1
y = numpy.random.random( ( N , M ) ) * 2 - 1
#math on arrays per element
np.exp(x)
np.multiply( x , y.T )
z = np.empty( shape = ( N , 1 ) , dtype = np.float32 )
k = np.empty( shape = ( N , 1 ) , dtype = np.float32 )

# Dot product of two arrays. for 2d arrays it is equivalent to matrix multiplication
# https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html
z.dot( k )


#######################################
############## pandas #################
#######################################

import pandas
features = pandas.read_csv('./features.csv', index_col='match_id')

#empty cells count:
for column in features:
    dif = len( features.index ) - features[ column ].count()

#replace empty cells
features.replace( '' , numpy.nan , inplace=True )
features.fillna( 0,inplace=True)

#filter columns
ignore_labels = ["radiant_win" , "match_id"]
X = features.select( lambda x: not x in ignore_labels , axis=1 ).as_matrix()





































#######################################

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
