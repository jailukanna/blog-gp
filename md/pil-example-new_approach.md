---
title: pil example new approach (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'new approach'


Modules used in program: 
* `import PIL`
* `import tkinter`

## python new approach

Python pil example: new approach

```python
import tkinter
from tkinter import messagebox
from PIL import Image, ImageDraw
import PIL

class Paint(object):

    def __init__(self):
        self.root = tkinter.Tk()
        self.width = 600
        self.height = 600
        self.center = self.height//2
        self.white = (255, 255, 255)

        self.canvas = tkinter.Canvas(self.root, bg='white', width=600, height=600)
        self.canvas.grid(row=1, columnspan=5)
        self.dropdownmenu()
        self.setup()
        self.draws()
        self.root.mainloop()

    def setup(self):
        self.old_x = None
        self.old_y = None
        self.canvas.bind('<B1-Motion>', self.paint)
        self.canvas.bind('<ButtonRelease-1>', self.reset)


    def paint(self, event):
        if self.old_x and self.old_y:
            self.canvas.create_line(self.old_x, self.old_y, event.x, event.y, smooth="yes")
            self.draw.line([self.old_x, self.old_y, event.x, event.y], fill="black")
        self.old_x = event.x
        self.old_y = event.y



    def save(self):
        self.f = messagebox.askquestion('', 'Save image?')
        if self.f == 'yes':
            filename = input("Enter filename: ")
            self.image1.save(filename)


    def mpouton(self):
        self.a = messagebox.askquestion('Warning!', 'Delete all?')
        if self.a == 'yes':
            self.canvas.delete('all')


    def reset(self, event):
        self.old_x, self.old_y = None, None


    def draws(self):
        self.image1 = PIL.Image.new("RGB", (self.width, self.height), self.white)
        self.draw = ImageDraw.Draw(self.image1)

    def dropdownmenu(self):
        self.ddmenu = tkinter.Menu(self.root)
        self.root.config(menu=self.ddmenu)
        self.sub_menu = tkinter.Menu(self.ddmenu)
        self.ddmenu.add_cascade(label="File", menu=self.sub_menu)
        self.sub_menu.add_command(label="Save", command=self.save)
        self.sub_menu.add_command(label="Clear Screen", command=self.mpouton)


if __name__ == '__main__':
    Paint()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
