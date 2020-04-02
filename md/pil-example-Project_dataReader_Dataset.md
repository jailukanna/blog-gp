---
title: pil example Project dataReader Dataset (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project dataReader Dataset'

Functions in program: 
* `def get_siam_set(list):`

Modules used in program: 
* `import torch.nn.functional as F`
* `import torch.nn as nn`
* `import PIL.ImageOps`
* `import torch`
* `import random`
* `import numpy as np`
* `import torchvision.utils`
* `import matplotlib.pyplot as plt`
* `import torchvision.transforms as transforms`
* `import torchvision.datasets as dset`
* `import torchvision`
* `import pandas as pd`
* `import shlex`

## python Project dataReader Dataset

Python pil example: Project dataReader Dataset

```python
import shlex
import pandas as pd
import torchvision
import torchvision.datasets as dset
import torchvision.transforms as transforms
from torch.utils.data import DataLoader,Dataset
import matplotlib.pyplot as plt
import torchvision.utils
import numpy as np
import random
from PIL import Image, ImageChops
import torch
from torch.autograd import Variable
import PIL.ImageOps
import torch.nn as nn
from torch import optim
import torch.nn.functional as F

from dataReader.Relabeler import train_test_split
from util.config import IMAGE_DIR, GrayScale, DATA_DIR


class SiameseNetworkDataset(Dataset):

    def __init__(self, triple_list, transform=None, should_invert=False):
        self.transform = transform
        self.should_invert = should_invert
        self.lastLabel = 1
        self.ref_list = triple_list[0]
        self.sim_list = triple_list[1]
        self.label_list = triple_list[2]

    def __getitem__(self, index):
        i = index
        #while (self.label_list[index] == self.lastLabel):
        #   index = index +1
        self.lastLabel = self.label_list[index]
        img0_tuple = self.ref_list[index]
        img1_tuple = self.sim_list[index]
        stable = Image.open('/home/jasper/Documents/BP_Jasp/project/util/stable.png' )
        img0 = Image.open(IMAGE_DIR +img0_tuple)
        img1 = Image.open(IMAGE_DIR +img1_tuple)

        diff = ImageChops.subtract(img0, img1)
        diff = diff.point(lambda i: i * 5)
        w, h = diff.size
        area = (9, 70, 115, 229)
        diff = diff.crop(area)
        if (GrayScale):
            diff= diff.convert("L")
            stable = stable.convert("L")

        img1 = ImageChops.subtract(img0, img1)
        if self.should_invert:
            stable = PIL.ImageOps.invert( stable)
            diff = PIL.ImageOps.invert( diff)

        if self.transform is not None:
            stable = self.transform(stable)
            diff = self.transform(diff)

        return stable, diff, torch.from_numpy(np.array([int(self.label_list[index])], dtype=np.float32))

    def __len__(self):
        return len(self.ref_list)



def get_siam_set(list):
    return SiameseNetworkDataset(triple_list=list,
                          transform=transforms.Compose([
                              transforms.Scale(30),
                              transforms.CenterCrop(28),
                              transforms.ToTensor()
                          ])
                          , should_invert=False)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
