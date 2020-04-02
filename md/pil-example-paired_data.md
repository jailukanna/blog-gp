---
title: pil example paired data (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'paired data'


Modules used in program: 
* `import torch`
* `import os.path`
* `import os`
* `import torch.utils.data as data`
* `import random`
* `import numpy as np`

## python paired data

Python pil example: paired data

```python
import numpy as np
import random
import torch.utils.data as data
from PIL import Image
import os
import os.path
from torchvision import transforms
import torch
from utility_functions import *
from image_folder import *

class PairedData(object):
    def __init__(self, return_paths):
        self.return_paths = return_paths
        self.iter = None

    def __iter__(self):
        pass

    def __next__(self):
        pass


class AlignedPairedData(PairedData):
    def __init__(self, data_loader, return_paths):
        super(AlignedPairedData, self).__init__(return_paths)
        self.data_loader = data_loader
        self.data_loader_iter = None
        self.stop = False

    def __iter__(self):
        self.data_loader_iter = iter(self.data_loader)
        self.iter = 0

    def __next__(self):
        if self.return_paths:
            data, data_path = next(self.data_loader_iter)
            self.iter += 1
            return {'A': data[0], 'B': data[1], 'A_Path': data_path[0], 'B_Path': data_path[1]}

        else:
            data = next(self.data_loader_iter)
            self.iter += 1
            return {'A': data[0], 'B': data[1]}


class UnalignedPairedData(PairedData):
    """Class to combine two items of 2 datasets"""
    def __init__(self, data_loader_a, data_loader_b, return_paths=True):
        """Function to initialize and create class variables"""
        super(UnalignedPairedData, self).__init__(return_paths)
        self.dataLoaderA = data_loader_a
        self.dataLoaderB = data_loader_b
        self.dataLoaderAIter = None
        self.dataLoaderBIter = None
        self.stopA = False
        self.stopB = False

    def __iter__(self):
        """
        Function to iterate through datasets
        :return: self
        """
        self.stopA = False
        self.stopB = False

        self.dataLoaderAIter = iter(self.dataLoaderA)
        self.dataLoaderBIter = iter(self.dataLoaderB)
        self.iter = 0

        return self

    def __next__(self):
        """
        Function to get next items of datasets
        :return: Dictionary containing the items
        """
        if self.return_paths:
            a, a_path = None, None
            b, b_path = None, None

            try:
                a, a_path = next(self.dataLoaderAIter)
            except StopIteration:
                if a is None or a_path is None:
                    self.stopA = True
                    self.dataLoaderAIter = iter(self.dataLoaderA)
                    a, a_path = next(self.dataLoaderAIter)

            try:
                b, b_path = next(self.dataLoaderBIter)
            except StopIteration:
                if b is None or b_path is None:
                    self.stopB = True
                    self.dataLoaderBIter = iter(self.dataLoaderB)
                    b, b_path = next(self.dataLoaderBIter)

            if self.stopA and self.stopB:
                self.stopA = False
                self.stopB = False
                raise StopIteration()
            else:
                self.iter += 1
                return {'A': a, 'B': b, 'A_Path': a_path, 'B_Path': b_path}
        else:
            a = None
            b = None

            try:
                a = next(self.dataLoaderAIter)
            except StopIteration:
                if a is None:
                    self.stopA = True
                    self.dataLoaderAIter = iter(self.dataLoaderA)
                    a = next(self.dataLoaderAIter)

            try:
                b = next(self.dataLoaderBIter)
            except StopIteration:
                if b is None:
                    self.stopB = True
                    self.dataLoaderBIter = iter(self.dataLoaderB)
                    b = next(self.dataLoaderBIter)

            if self.stopA and self.stopB:
                self.stopA = False
                self.stopB = False
                raise StopIteration()
            else:
                self.iter += 1
                return {'A': a, 'B': b}


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
