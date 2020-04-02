---
title: pil example ImageProcessing (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ImageProcessing'

Functions in program: 
* `def clicked():`
* `def blur_image():`

Modules used in program: 
* `import PIL.Image, PIL.ImageTk`
* `import tkinter`
* `import cv2`

## python ImageProcessing

Python pil example: ImageProcessing

```python
import cv2
import tkinter
import PIL.Image, PIL.ImageTk
from tkinter.filedialog import askopenfilename
from tkinter import *
from config import Config
# Create a window

Config.filename = "C:/Users/huyen/Downloads/1.png"

def blur_image():
    global photo
    global cv_img
    cv_img = cv2.cvtColor(cv2.imread(Config.filename), cv2.COLOR_BGR2RGB)
    cv_img = cv2.blur(cv_img, (20,20))
    height, width, no_channels = cv_img.shape
    canvas = tkinter.Canvas(window, width = width, height = height)
    photo = PIL.ImageTk.PhotoImage(image = PIL.Image.fromarray(cv_img))
    canvas.create_image(0, 0, image=photo, anchor=tkinter.NW)
    canvas.grid(column=4, row=0)
    
    
window = tkinter.Tk()
def clicked():
    Tk().withdraw()
    global filename
    filename = askopenfilename()
    Config.filename = filename
    global photo_2
    global cv_img_2
    cv_img_2 = cv2.cvtColor(cv2.imread(filename), cv2.COLOR_BGR2RGB)
    height, width, no_channels = cv_img_2.shape
    canvas_2 = tkinter.Canvas(window, width = width, height = height)
    photo_2 = PIL.ImageTk.PhotoImage(image = PIL.Image.fromarray(cv_img_2))
    canvas_2.create_image(0, 0, image=photo_2, anchor=tkinter.NW)
    canvas_2.grid(column=1, row=0)
    
btn = Button(window, text="Lấy ảnh giùm bố",command=clicked)

btn.grid(column=0, row=0)
 
#Ảnh sau khi lấy.


#ảnh sau khi xử lý




#photo = PIL.ImageTk.PhotoImage(image = PIL.Image.fromarray(cv_img))
#canvas.create_image(0, 0, image=photo, anchor=tkinter.NW)

btn_blur=tkinter.Button(window, text="Blur", width=10, command=blur_image)
#btn_blur.pack(anchor=tkinter.CENTER, expand=True)
btn_blur.grid(column=3, row=0)

window.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
