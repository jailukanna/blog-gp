---
title: pil example Project nets AutoEncoder (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project nets AutoEncoder'


Modules used in program: 
* `import torchvision.datasets as dset`
* `import pandas as pd`
* `import os`
* `import torchvision`
* `import torch`

## python Project nets AutoEncoder

Python pil example: Project nets AutoEncoder

```python
import torch
import torchvision
from torch import nn
from torch.autograd import Variable
from torch.utils.data import DataLoader
from torchvision import transforms
from torchvision.utils import save_image
from torchvision.datasets import MNIST
import os
import pandas as pd
import torchvision.datasets as dset

from dataReader.DataReader import load_checkpoint, save_checkpoint
from dataReader.Relabeler import train_test_split, DATA_DIR
from dataReader.dataset import get_siam_set
from util.config import train_batch_size

from utils.Visualize import to_img


class autoencoder(nn.Module):
    def __init__(self):
        super(autoencoder, self).__init__()
        self.encoder = nn.Sequential(
            nn.Conv2d(1, 16, 3, stride=3, padding=1),  # b, 16, 10, 10
            nn.ReLU(True),
            nn.MaxPool2d(2, stride=2),  # b, 16, 5, 5
            nn.Conv2d(16, 8, 3, stride=2, padding=1),  # b, 8, 3, 3
            nn.ReLU(True),
            nn.MaxPool2d(2, stride=1)  # b, 8, 2, 2
        )
        self.decoder = nn.Sequential(
            nn.ConvTranspose2d(8, 16, 3, stride=2),  # b, 16, 5, 5
            nn.ReLU(True),
            nn.ConvTranspose2d(16, 8, 5, stride=3, padding=1),  # b, 8, 15, 15
            nn.ReLU(True),
            nn.ConvTranspose2d(8, 1, 2, stride=2, padding=1),  # b, 1, 28, 28
            nn.Tanh()
        )

    def forward(self, x):
        x = self.encoder(x)
        x = self.decoder(x)
        return x

    def name(self):
        return "autoEncoder 1.0"

    def dummyInput(self):
        return Variable(torch.rand(1, 1, 28, 28))

    def trainBatch(self, data, optimizer, criterion):
        img0, img, label1 = data
        img = Variable(img).cuda()
        # ===================forward=====================
        output = self(img)
        loss = criterion(output, img)
        # ===================backward====================
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        return loss

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
