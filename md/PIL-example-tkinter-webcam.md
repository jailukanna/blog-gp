---
title: PIL example tkinter-webcam (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'tkinter-webcam'

Functions in program: 
* `def show_frame():`

Modules used in program: 
* `import cv2`
* `import pytesseract`
* `import PIL`

## python tkinter-webcam

Python PIL example: tkinter-webcam

```python
import PIL
from PIL import Image,ImageTk
import pytesseract
import cv2
from tkinter import *

width, height = 1920, 1080
cap = cv2.VideoCapture("0")
cap.set(cv2.CAP_PROP_FRAME_WIDTH, width)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, height)

root = Tk()
root.bind('<Escape>', lambda e: root.quit()) 
lmain = Label(root)
lmain.pack()

def show_frame():
    ret, frame = cap.read()
    if ret:
        frame = cv2.flip(frame, 1)
        cv2image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGBA)
        img = PIL.Image.fromarray(cv2image)
        imgtk = ImageTk.PhotoImage(image=img)
        lmain.imgtk = imgtk
        lmain.configure(image=imgtk)
        lmain.after(10, show_frame)

show_frame()
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
