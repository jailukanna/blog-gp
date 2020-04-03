---
title: matplotlib example test stn (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test stn'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import torch.nn as nn`
* `import numpy as np`
* `import torch`
* `import matplotlib`

## python test stn

Python matplotlib example: test stn

```python
# coding: utf-8

from __future__ import print_function

import matplotlib
matplotlib.use('Agg')

import torch
import numpy as np
import torch.nn as nn
from torch.autograd import Variable
from modules.stn import STN
from modules.gridgen import CylinderGridGen, AffineGridGen
from PIL import Image
from matplotlib import mlab
import matplotlib.pyplot as plt


# In[2]:

img = Image.open('cat.jpg').convert('RGB')
img = np.array(img)/255.0
#plt.imshow(img)


# In[3]:

img_batch = np.expand_dims(img, 0)
inputImages = torch.from_numpy(img_batch.astype(np.float32))
inputImages.size()
s = STN(layout = "BCHW")
g = AffineGridGen(328, 582)
input = Variable(torch.from_numpy(np.array([[[1, 0.5, 0], [0.5, 1, 0]]], dtype=np.float32)), requires_grad = True)
out = g(input)

input1 = Variable(inputImages)

input1 = input1.permute(0,3,1,2).contiguous()
out = out.permute(0,3,1,2).contiguous()

res = s(input1, out)

print(res.size())

res_np = res[0].cpu().data.numpy().transpose(1,2,0)
print(res_np.shape)
res_np = (res_np * 255).astype(np.uint8)

im = Image.fromarray(res_np)
im.save("res.jpeg")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
