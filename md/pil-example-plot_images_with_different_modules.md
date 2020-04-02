---
title: pil example plot images with different modules (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'plot images with different modules'


Modules used in program: 
* `import os`
* `import torch`
* `import cv2`
* `import matplotlib.image as mpimg `
* `import matplotlib.pyplot as plt `
* `import numpy as np`

## python plot images with different modules

Python pil example: plot images with different modules

```python
import numpy as np
import matplotlib.pyplot as plt 
# image modules
from PIL import Image
import matplotlib.image as mpimg 
import cv2
# PyTorch
import torch
from torch.utils import data
from torchvision import transforms
# other module
import os

my_dpi = 200
fig = plt.figure(figsize=(6, 6), dpi=my_dpi)

# ============ AX1 ============ 
# PIL Image
ax1 = fig.add_subplot(2, 3, 1)
ax1.set_title("PIL Image")
ax1.set_xlabel('X label')
ax1.set_ylabel('Y label')
ax1.set_xticks([])
ax1.set_yticks([])
pil_img = Image.open(os.path.join('my_data', 'img1.jpg'))
ax1.imshow(pil_img)

# ============ AX2 ============ 
# mpimg image
ax2 = fig.add_subplot(2, 3, 2)
ax2.set_title("mpimg image")
ax2.set_xticks([])
ax2.set_yticks([])
mpimg_img = mpimg.imread(os.path.join('my_data', 'img2.jpg')) 
ax2.imshow(mpimg_img)

# ============ AX3 ============ 
# CV2 image (default)
ax3 = fig.add_subplot(2, 3, 3)
ax3.set_title("CV2 image (default)")
ax3.set_xticks([])
ax3.set_yticks([])
opencv_img = cv2.imread(os.path.join('my_data', 'img3.jpg'))
ax3.imshow(opencv_img)

# ============ AX4 ============ 
# CV2 image (transform)
ax4 = fig.add_subplot(2, 3, 4)
ax4.set_title("CV2 image (transform)")
ax4.set_xticks([])
ax4.set_yticks([])
cv2_img = cv2.imread(os.path.join('my_data', 'img3.jpg'))
mod_cv2_img = cv2.cvtColor(cv2_img, cv2.COLOR_BGR2RGB) 
ax4.imshow(mod_cv2_img)

# ============ AX5 ============ 
# CV2 image (transform)
ax5 = fig.add_subplot(2, 3, 5)
ax5.axis('off')       
cv2_img = cv2.imread(os.path.join('my_data', 'img3.jpg'))
mod_cv2_img = cv2.cvtColor(cv2_img, cv2.COLOR_BGR2RGB) 
ax5.imshow(mod_cv2_img)

# ============ AX6 ============ 
# PIL image. With PyTorch, tensorise and de-tensorise
ax6 = fig.add_subplot(2, 3, 6)
ax6.set_title("PIL image (tensor)")
ax6.set_xlabel('X label')
ax6.set_ylabel('Y label')
ax6.set_xticklabels('')
ax6.set_yticklabels('')
pil_image = Image.open(os.path.join('my_data', 'img1.jpg'))
tensor_image = transforms.ToTensor()(pil_image)
de_tensor_image = transforms.ToPILImage()(tensor_image)
ax6.imshow(de_tensor_image)

fig.savefig("saved_images.jpg", dpi=DPI, bbox_inches='tight')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
