---
title: PIL example ir (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ir'

Functions in program: 
* `def forward(model, x, t):`
* `def getTrainingData(img):`
* `def to_np(imgpath):`
* `def showImageArray(array, fmt='jpeg'):`

Modules used in program: 
* `import PIL.Image`
* `import io`
* `import chainer.links as L`
* `import chainer.functions as F`
* `import chainer`
* `import matplotlib.pyplot as plt`
* `import numpy`

## python ir

Python PIL example: ir

```python
import numpy
import matplotlib.pyplot as plt

import chainer
from chainer import cuda
import chainer.functions as F
import chainer.links as L
from chainer import optimizers
from chainer import cuda, Function, gradient_check, Variable, optimizers, serializers, utils
from chainer import Link, Chain, ChainList

import io
from IPython.display import clear_output, Image, display
from scipy.misc import imresize
import PIL.Image

def showImageArray(array, fmt='jpeg'):
    array = numpy.uint8(numpy.clip(array, 0, 255))
    f = io.BytesIO()
    
    PIL.Image.fromarray(array).save(f, fmt,  quality=100, optimize=True)

    display(Image(data=f.getvalue()))
#     pilImg = PIL.Image.fromarray(array)
#     pilImg.show()
#     display(Image(data=array))


def to_np(imgpath):
    return numpy.float32(PIL.Image.open(imgpath))

def getTrainingData(img):
    print(img.shape)
    
    rgbs = []
    indices = []
    
    width = img.shape[0]
    height = img.shape[1]
    
    for i in range(width):
        for j in range(height):
            _s = float(width)
            
            x = (i - _s / 2.0) / _s
            y = (j - _s / 2.0) / _s
            rgb = img[i, j]
            
            rgbs.append(rgb)
            
            indices.append([x, y])
    
    rgbs = numpy.array(rgbs, dtype=numpy.float32)
    indices = numpy.array(indices, dtype=numpy.float32)
    
    x_data = Variable(indices)
    y_data = Variable(rgbs / 255.0 - 0.5)
#     y_data = Variable(rgbs)
    
    return x_data, y_data


size = 50

img = imresize(to_np("./cat.jpeg"), (size, size))

# print(img)
showImageArray(img)
x_data, y_data =  getTrainingData(img)

model = chainer.Chain(l1=L.Linear(2, 20, wscale=2),
                        l2 = L.Linear(20, 20, wscale=1),
                        l3=L.Linear(20, 20, wscale=2),
                        l4=L.Linear(20, 20, wscale=1),
                        l5=L.Linear(20, 20, wscale=2),
                        l6=L.Linear(20, 20, wscale=1),
                        l7=L.Linear(20, 20, wscale=1),
                        l8=L.Linear(20, 20, wscale=1), 
                        l13=L.Linear(20, 3, wscale=2))

def forward(model, x, t):
    hidden = F.relu(model.l1(x))
    hidden = F.relu(model.l2(hidden))
    hidden = F.relu(model.l3(hidden))
    hidden = F.relu(model.l4(hidden))
    hidden = F.relu(model.l5(hidden))
    hidden = F.relu(model.l6(hidden))
    hidden = F.relu(model.l7(hidden))
    hidden = F.relu(model.l8(hidden))
    hidden = model.l13(hidden)
    
    return F.mean_squared_error(hidden, t), hidden

optimizer = optimizers.SGD(lr=0.01)
optimizer.setup(model)


for i in range(1000000):
    optimizer.zero_grads()
    loss, hidden = forward(model, x_data, y_data)
    
#     print(i)
    
    loss.backward()
    optimizer.update()
    
    if i  % 100 == 0:
#         print((hidden.data + 0.5) * 255.)
        img2 = hidden.data.reshape((50,50,3))
#         print((img2 + 0.5) * 255.)
        showImageArray((img2 + 0.5) * 255.)
#         showImageArray((hidden.data + 0.5) * 255.)
        clear_output(wait=True)

print("done")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
