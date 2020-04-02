---
title: pil example normalize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'normalize'


## python normalize

Python pil example: normalize

```python
"""
Reference: https://pytorch.org/docs/master/torchvision/models.html
"""

from urllib.request import urlopen
from PIL import Image

from torchvision import transforms

# Read an example image
url = 'https://raw.githubusercontent.com/quanguet/images/master/cat.jpg'
img = Image.open(urlopen(url)) # PIL Image


# Crop
crop = transforms.transforms.RandomCrop((240, 240))
crop(img) # PIL Image

# ToTensor

totensor = transforms.ToTensor()
"""
Converts a PIL Image or numpy.ndarray (H x W x C) in the range [0, 255] 
to a torch.FloatTensor of shape (C x H x W) in the range [0.0, 1.0] 
if the PIL Image belongs to one of the modes (L, LA, P, I, F, RGB, YCbCr, RGBA, CMYK, 1) 
or if the numpy.ndarray has dtype = np.uint8
"""

type(totensor(img)) # torch.Tensor

# Normalize
"""
Normalize the range [0.0, 1.0] to [-1.0 1.0]
Which channels: [0.485/R, 0.456/G, 0.406/B]
"""
normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406],
                                 std=[0.229, 0.224, 0.225])

normalize(img) # Error since input requires torch.Tensor
type(normalize(totensor(img))) # torch.Tensor

# In Pytorch pretrained models, the standard transforms is as below
# supposed the image is PIL Image in RGB

transforms.Compose([
  transforms.ToTensor(),
  transforms.Normalize(mean=[0.485, 0.456, 0.406],std=[0.229, 0.224, 0.225])])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
