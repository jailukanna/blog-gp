---
title: pil example anchor vis (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'anchor vis'


Modules used in program: 
* `import torchvision.transforms.functional as F`

## python anchor vis

Python pil example: anchor vis

```python
from PIL import Image, ImageFont, ImageDraw, ImageEnhance
import torchvision.transforms.functional as F

# image is the tensor defined as torch.zeros((3, 800, 800)).float()

## visualize all centers and boxes for 1 position
pil_image = F.to_pil_image(image)
draw = ImageDraw.Draw(pil_image)
for anchor in anchor_base:
    draw.rectangle(((anchor[1], anchor[0]), (anchor[3], anchor[2])))
    
draw = ImageDraw.Draw(pil_image)
for center in centers:
    draw.point((center[1], center[0]), fill="blue")
    
pil_image.show()

## visualize all boxes
pil_image = F.to_pil_image(image)
draw = ImageDraw.Draw(pil_image)
for anchor in anchors:
    draw.rectangle(((anchor[1], anchor[0]), (anchor[3], anchor[2])))
pil_image.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
