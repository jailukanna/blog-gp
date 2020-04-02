---
title: pil example Project utils Visualize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project utils Visualize'

Functions in program: 
* `def to_img(x):`
* `def show_plot(iteration,loss):`
* `def imshow(img,text=None,should_save=False):`
* `def diff(img, img1):  # returns just the difference of the two images`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python Project utils Visualize

Python pil example: Project utils Visualize

```python
import numpy as np
import matplotlib.pyplot as plt



def diff(img, img1):  # returns just the difference of the two images
    return cv2.absdiff(img, img1)


def imshow(img,text=None,should_save=False):
    npimg = img.numpy()
    plt.axis("off")
    if text:
        plt.text(75, 8, text, style='italic',fontweight='bold',
            bbox={'facecolor':'white', 'alpha':0.8, 'pad':10})
    plt.imshow(np.transpose(npimg, (1, 2, 0)))
    plt.show()

def show_plot(iteration,loss):
    plt.plot(iteration,loss)
    plt.show()




def to_img(x):
    x = 0.5 * (x + 1)
    x = x.clamp(0, 1)
    x = x.view(x.size(0), 1, 28, 28)
    return x

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
