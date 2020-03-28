---
title: tkinter example loader (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'loader'


Modules used in program: 
* `import tkinter as tk`

## python loader

Python tkinter example: loader

```python
import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageDraw


filetypes = (
        ("PNG", "*.png"),
        ("JPEG", "*.jpg"),
        ("BMP", "*.bmp")
)

root = tk.Tk()
image_name = tk.filedialog.askopenfilename(filetypes=filetypes)
im = Image.open(image_name)
width, height = im.size

if __name__ == '__main__':
    from negative import negative
    negative(im)

root.title('Shot Viewer')
w, h, x, y = width, height, 0, 0
root.geometry("%dx%d+%d+%d" % (w, h, x, y))

# Loading Image on tkinter
photo = tk.PhotoImage(file=image_name)
photo_label = tk.Label(image=photo)
photo_label.grid()
photo_label.image = photo

root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
