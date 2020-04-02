---
title: pil example Project nets SiamNet (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project nets SiamNet'


Modules used in program: 
* `import torchvision.datasets as dset`
* `import pandas as pd`
* `import os`
* `import torchvision`
* `import torch`

## python Project nets SiamNet

Python pil example: Project nets SiamNet

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

from utils.Visualize import to_img, diff


class siamAutoencoder(nn.Module):
    def __init__(self):

        super(siamAutoencoder, self).__init__()
        self.encoder = nn.Sequential(
            nn.Conv2d(1, 16, 3, stride=3, padding=1),  # b, 16, 10, 10
            nn.ReLU(True),
            nn.MaxPool2d(2, stride=2),  # b, 16, 5, 5
            nn.Conv2d(16, 8, 3, stride=2, padding=1),  # b, 8, 3, 3
            nn.ReLU(True),
            nn.MaxPool2d(2, stride=1)  # b, 8, 2, 2
        )
        # self.encoder = load_checkpoint(self.encoder)

        self.fc1 = nn.Sequential(
            nn.Linear(8 * 2 * 2, 16),
            nn.ReLU(inplace=True),

            nn.Linear(16, 8),
            nn.ReLU(inplace=True),

            nn.Linear(8, 1),
        )

    def forward_once(self, x):
        x = self.encoder(x)
        x = x.view(x.size()[0], -1)
        x = self.fc1(x)
        return x


    def forward(self, input1, input2):
        output1 = self.forward_once(input1)
        output2 = self.forward_once(input2)
        return output1, output2


    def name(self):
        return "SiamNet 1.0"

    def dummyInput(self):
        return Variable(torch.rand(1, 8, 2, 2))

    def trainBatch(self, data, optimizer, criterion):
        img0, img1, label1 = data
        diff1 = img0 - img0
        diff2 = diff(np.asarray(img1), np.asarray(img0))
        diff1, diff2 = Variable(diff1).cuda(), Variable(torch.FloatTensor(diff2)).cuda()
        img0, img1, label = Variable(img0).cuda(), Variable(img1).cuda(), Variable(label1).cuda()
        output1, output2 = self(diff1, diff2)
        optimizer.zero_grad()
        loss_contrastive = criterion(output1, output2, label)
        loss_contrastive.backward()
        optimizer.step()
        return loss_contrastive.data[0]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
