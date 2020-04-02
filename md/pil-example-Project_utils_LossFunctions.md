---
title: pil example Project utils LossFunctions (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project utils LossFunctions'


Modules used in program: 
* `import torch.nn.functional as F`
* `import torch.nn as nn`
* `import numpy.random as rng`
* `import PIL.ImageOps`
* `import torch`
* `import random`
* `import numpy as np`
* `import torchvision.utils`
* `import matplotlib.pyplot as plt`
* `import torchvision.transforms as transforms`
* `import torchvision.datasets as dset`
* `import torchvision`

## python Project utils LossFunctions

Python pil example: Project utils LossFunctions

```python
import torchvision
import torchvision.datasets as dset
import torchvision.transforms as transforms
from keras.layers import Dense
from keras.layers import merge
from torch.utils.data import DataLoader,Dataset
import matplotlib.pyplot as plt
import torchvision.utils
import numpy as np
import random
from PIL import Image
import torch
from keras import backend as K
from torch.autograd import Variable
import PIL.ImageOps
import numpy.random as rng
import torch.nn as nn
from torch import optim
import torch.nn.functional as F






class ContrastiveLoss(torch.nn.Module):
    """
    Contrastive loss function.
    Based on: http://yann.lecun.com/exdb/publis/pdf/hadsell-chopra-lecun-06.pdf
    """

    def __init__(self, margin=2.0):
        super(ContrastiveLoss, self).__init__()
        self.margin = margin

    def forward(self, output1, output2, label):
        euclidean_distance = F.pairwise_distance(output1, output2)
        loss_contrastive = torch.mean((1-label) * torch.pow(euclidean_distance, 2) +
                                      (label) * torch.pow(torch.clamp(self.margin - euclidean_distance, min=0.0), 2))


        return loss_contrastive

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
