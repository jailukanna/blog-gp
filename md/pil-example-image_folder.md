---
title: pil example image folder (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image folder'


Modules used in program: 
* `import torch`
* `import os.path`
* `import os`
* `import torch.utils.data as data`
* `import random`
* `import numpy as np`

## python image folder

Python pil example: image folder

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

class ImageFolder(data.Dataset):
    """Class for handling image load process and transformations"""

    def __init__(self, image_path, options, transform=None, return_paths=True,
                 loader=default_loader_unaligned):
        """
        Function to create the dataset and initialize the class variables
        :param image_path: path containing image-files
        :param options: class containing all options (args of BaseOptions or subclass)
        :param transform: transformation to apply on the Image after loading it
        :param return_paths: Boolean, True if paths should be returned alongside images , False if only images
        :param loader: function to load and resize images
        """
        imgs = make_dataset(image_path)
        if len(imgs) == 0:
            raise(RuntimeError("Found 0 images in: " + image_path + "\n"
                                                                    "Supported image extensions are: " + ",".join(IMG_EXTENSIONS)))

        self.root = image_path
        self.imgs = imgs
        self.transform = transform
        self.return_paths = return_paths
        self.loader = loader
        self.options = options

    def __getitem__(self, index):
        """
        Function to get certain item in dataset
        :param index: index of dataset-list
        :return: item in dataset with given index
        """
        path = self.imgs[index]
        img = self.loader(path, self.options.imageSize, self.options.inputNc)
        if self.transform is not None:
            img = self.transform(img)
        if self.return_paths:
            return img, path
        else:
            return img

    def __len__(self):
        """Function to get number of items in dataset"""
        return len(self.imgs)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
