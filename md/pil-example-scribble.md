---
title: pil example scribble (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'scribble'


Modules used in program: 
* `import tkinter as tk`
* `import numpy as np`

## python scribble

Python pil example: scribble

```python
import numpy as np
from PIL import Image, ImageTk, ImageDraw
import tkinter as tk

class simplePaint():
    '''Simple GUI to draw foreground and background markers on an image.
     Invoke by creating an object with `D = simplePaint(img)` where img is a PIL image or numpy ndarray.
     Key controls: 'f': foreground, 'b': background, '+'/'-': increase/decrease pen size.
     Close the window to quit. The label mask is stored in the object variable `D.msk`.
    '''

    DEFAULT_COLOR = 'red'
    DEFAULT_LABEL = 2
    DEFAULT_RADIUS = 5

    def __init__(self, img):
        if isinstance(img, Image.Image):
            self.img = img
        elif isinstance(img, np.ndarray):
            self.img = Image.fromarray(img)
        else:
            raise ValueError('Image type not recognized. Use PIL.Image or ndarray.')

        self.canvas_width = self.img.width
        self.canvas_height = self.img.height
        self.nlab = 2
        self.radius = self.DEFAULT_RADIUS
        self.col_pen = self.DEFAULT_COLOR
        self.current_label = self.DEFAULT_LABEL
        self.msk = Image.new('I',self.img.size)
        self.draw_msk = ImageDraw.Draw(self.msk)
        self.master = tk.Tk() 
        self.setup()
        self.master.mainloop() 

    def setup(self):
        self.master.title( "Interactive label painting." ) 
        self.w = tk.Canvas(self.master, width=self.canvas_width,
                height=self.canvas_height) 
        self.w.pack(expand = tk.YES, fill = tk.BOTH) 
        self.image = ImageTk.PhotoImage(self.img)
        self.w.create_image(0,0,image=self.image,anchor=tk.NW)
        self.master.bind( "<Key>", self.onkey ) 
        self.w.bind( "<B1-Motion>", self.paint ) 
        self.w.bind( "<Button-1>", self.paint ) 
        self.w.bind( "<Destroy>", self.onquit ) 

    def paint(self,event): 
        r = self.radius
        x1, y1 = ( event.x - r ), ( event.y - r ) 
        x2, y2 = ( event.x + r ), ( event.y + r ) 
        self.draw_msk.ellipse([x1,y1,x2,y2], fill = self.current_label)
        self.w.create_oval( x1, y1, x2, y2, fill = self.col_pen, 
                outline=self.col_pen )   
    
    def onkey(self,event):
        if event.char == 'f':
            self.col_pen = 'red'
            self.current_label = 2
        if event.char == 'b':
            self.col_pen = 'blue'
            self.current_label = 1
        if event.char == '+':
            self.radius += 1
            self.radius = np.minimum(self.radius,50)
        if event.char == '-':
            self.radius -= 1
            self.radius = np.maximum(self.radius,0)

    def onquit(self,event):
        self.msk = np.array(self.msk)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
