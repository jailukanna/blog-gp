---
title: PIL example Project main (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Project main'

Functions in program: 
* `def run():`

Modules used in program: 
* `import os`
* `import torch.nn.functional as F`
* `import torch.nn as nn`
* `import pandas as pd`
* `import PIL.ImageOps`
* `import torch`
* `import random`
* `import numpy as np`
* `import torchvision.utils`
* `import matplotlib.pyplot as plt`
* `import torchvision.transforms as transforms`
* `import torchvision.datasets as dset`
* `import torchvision`
* `import cv2`

## python Project main

Python PIL example: Project main

```python
import cv2
import torchvision
import torchvision.datasets as dset
import torchvision.transforms as transforms
from torch.utils.data import DataLoader,Dataset
import matplotlib.pyplot as plt
import torchvision.utils
from torchvision.models import vgg16_bn

from Eval import get_accuracy
from Train import trainBatch
from dataReader.DataReader import get_triples, save_checkpoint, load_checkpoint, epoch
from dataReader.Relabeler import write_list, train_test_split, update_list
from logger import Logger
from nets.SiamNetwork import ContrastiveLoss, SiameseNetwork, SiameseNetworkRGB, SiameseNetwork2
from dataReader.dataset import SiameseNetworkDataset, get_siam_set
from nets.autoEncoder import siamAutoencoder
from nets.resnet import ResNet18, ResNetSiam
from util.config import DATA_DIR, IMAGE_DIR, train_batch_size, NUMBER_OF_EPOCH
from util.plot_image import show_plot, imshow
import numpy as np
import random
from PIL import Image
import torch
from torch.autograd import Variable
import PIL.ImageOps
import pandas as pd
import torch.nn as nn
from torch import optim
import torch.nn.functional as F
import os



currentEpoch=0

#validate using a test set
validate = True

#define a logger for tensorboard
log = Logger()


#define network to use
net = siamAutoencoder()
optimizer = optim.Adam(net.parameters(),lr = 0.0005 )
net = load_checkpoint(net,optimizer)


#todo modify logger
dummy_input = Variable(torch.rand(1, 1, 28, 28))
log.AddGraph(net,dummy_input.cuda())
criterion = ContrastiveLoss()



#load the train and test list
list = pd.read_csv('/home/jasper/Documents/BP_Jasp/data/pgbalancedList.csv', sep=',', header=None)
trainIndx, testIndx, valIndx = train_test_split(list,splits=[0.7, 0.1, 0.2])
siamese_dataset = get_siam_set(list[0:trainIndx])
testSet = np.transpose( list[trainIndx:trainIndx+testIndx])
triple_list = np.empty_like(testSet)
triple_list[:]= testSet
train_dataloader = DataLoader(siamese_dataset,
                        shuffle=True,
                        num_workers=8,
                        batch_size=train_batch_size)
test_dataset = get_siam_set(triple_list)
test_dataloader = DataLoader(test_dataset, num_workers=6, batch_size=1, shuffle=False)



def run():
    counter = []
    loss_history = []
    iteration_number = 0
    for epoch in range(currentEpoch, currentEpoch + NUMBER_OF_EPOCH):
        for i, data in enumerate(train_dataloader, 0):
            loss = net.trainBatch(data, optimizer, criterion)
            if i % 10 == 0:
                log.writeTB('loss', loss, iteration_number)
                if (validate):
                    accu = get_accuracy(net , test_dataloader, 1.5)
                    log.writeTB('testAccuracy', accu, iteration_number)
                    print("Epoch number {}\n Current loss {}\n accuracy  {}\n ".format(epoch, loss, accu))
                else:
                    print("Epoch number {}\n Current loss {}\n  ".format(epoch, loss))
                iteration_number += 10
                counter.append(iteration_number)
                loss_history.append(loss)
    save_checkpoint(net, epoch, optimizer)
    log.close()
    show_plot(counter, loss_history)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
