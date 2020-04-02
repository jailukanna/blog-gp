---
title: pil example Image01 jpg01 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Image01 jpg01'


Modules used in program: 
* `import tkinter as tk`

## python Image01 jpg01

Python pil example: Image01 jpg01

```python
#from __future__ import print_function
from PIL import Image, ImageTk
import tkinter as tk

im = Image.open('_IGP2599.JPG')
print(im.format, im.size, im.mode,im.info)

root = tk.Tk()
root.title('_IGP2599.JPG')

photo = ImageTk.PhotoImage(im)

l = tk.Label(root,image=photo)
l.pack()
l.photo = photo

#root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
