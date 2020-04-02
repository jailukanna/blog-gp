---
title: PIL example test%2520version model (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test%2520version model'

Functions in program: 
* `def fine_tune_model():`

Modules used in program: 
* `import torch`
* `import pytorch_colors as colors`

## python test%2520version model

Python PIL example: test%2520version model

```python
from torch import nn, optim
from torchvision import models
import pytorch_colors as colors
import torch
from random import randint
from torchvision import transforms


class RGB_model(nn.Module):
    def __init__(self):
        super(RGB_model, self).__init__()
        self.RGB = fine_tune_model()

    def forward(self, x):
        out = self.RGB(x)
        return out


class HSV_model(nn.Module):
    def __init__(self):
        super(HSV_model, self).__init__()
        self.HSV = fine_tune_model()

    def forward(self, x):
        out = colors.rgb_to_hsv(x)
        out = self.HSV(out)
        return out


class YCbCr_model(nn.Module):
    def __init__(self):
        super(YCbCr_model, self).__init__()
        self.YCbCr = fine_tune_model()

    def forward(self, x):
        out = colors.rgb_to_ycbcr(x)
        out = self.YCbCr(out)
        return out


class patch_model(nn.Module):
    def __init__(self):
        super(patch_model, self).__init__()
        self.patch = fine_tune_model()

    def forward(self, x):
        out = self.patch(x)
        return out


def fine_tune_model():
    model_ft = models.resnet18(pretrained=True)
    num_features = model_ft.fc.in_features
    model_ft.fc = nn.Linear(2048, 2)
    return model_ft


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
