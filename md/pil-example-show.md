---
title: pil example show (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'show'

Functions in program: 
* `def square_grid(x):`
* `def showtensor(t, fmt='jpeg'):`
* `def showarray(a, fmt='jpeg'):`

Modules used in program: 
* `import PIL.Image`

## python show

Python pil example: show

```python
from io import BytesIO
import PIL.Image
from IPython.display import clear_output, Image, display

def showarray(a, fmt='jpeg'):
    a = a - a.min()
    a = 255.*(a/a.max())
    a = np.uint8(np.clip(a, 0, 255))
    f = BytesIO()
    PIL.Image.fromarray(a).save(f, fmt)
    display(Image(data=f.getvalue()))

def showtensor(t, fmt='jpeg'):
    if t.dtype is not torch.uint8:
        t = t.to(torch.float)
        t = t - t.min()
        t = 255.*(t/t.max())
        t = torch.clamp(t, 0, 255).to(torch.uint8)
    f = BytesIO()
    PIL.Image.fromarray(t.numpy()).save(f, fmt)
    display(Image(data=f.getvalue()))
    
def square_grid(x):
    """Takes a 3d tensor of shape (n_images, width, height) and produces a grid of those images.
       If n_images has an integer square root, y will be square (sqrt(n_images)*width, sqrt(n_images)*height).
       If not, all images will be displayed in a column (n_images*width, height)."""
    from math import sqrt
    n, w, h = x.size()
    d = sqrt(float(n))
    if abs(d - round(d)) < 1e-6:
        d = int(d)
        y = x.view(d, d*w, h)
        y = torch.cat([y[i] for i in range(d)], 1)
    else:
        y = x.view(n*w, h)
    return y

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
