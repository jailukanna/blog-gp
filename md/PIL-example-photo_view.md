---
title: PIL example photo view (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'photo view'

Functions in program: 
* `def attach_menubar(window):`

Modules used in program: 
* `import PIL.ImageTk`
* `import PIL.Image`
* `import platform`
* `import tkFileDialog`

## python photo view

Python PIL example: photo view

```python
#!/usr/bin/env python
# coding:utf-8

from Tkinter import Tk, Frame, Menu, Label, PhotoImage, BOTH
import tkFileDialog
import platform

import PIL.Image
import PIL.ImageTk


class PhotoViewerFrame(Frame):
    """ phpto view window.
        if triple click on window then dialog is open.
    """
    def __init__(self, main_window=None):
        #super(PhotoViewer, self).__init__(main_window)
        Frame.__init__(self, main_window)
        self.master.title(u'photo viewer')
        self.init()
        self.pack()

    def init(self):
        image_info_label = self.image_info_label = Label(self, bg="#AFEEEE",
                                                            width=40, height=2)
        image_info_label.pack(pady=5, expand=True, fill=BOTH)
        self.view_image = PhotoImage()
        image_label = self.image_label = Label(self, image=self.view_image, 
                                                bg="#000000", width=300, 
                                                height=300, text="triple click")
        image_label.bind("<Triple-Button-1>", self.open)
        image_label.bind("<Button-3>", self.quit)
        image_label.pack()

    def open(self, event):
        file_name = tkFileDialog.askopenfilename()
        if file_name != "":
            image = PIL.Image.open(file_name)
            if image.mode == "1":
                # bitmap image
                # mode list : http://effbot.org/imagingbook/concepts.htm#mode
                self.view_image = PIL.ImageTk.BitmapImage(image, foreground="white")
            else:
                # photo image
                self.view_image = PIL.ImageTk.PhotoImage(image)
            self.image_label.config(image=self.view_image,
                                    width=self.view_image.width(),
                                    height=self.view_image.height())
            self.__add_image_info_text(image, file_name)

    def quit(self, event):
        self.master.destroy()

    def __add_image_info_text(self, image, file_name):
        delimiter = "/"
        if platform.system() == "Windows":
            delimiter = "\\"
        file_name_piece = file_name.split(delimiter)
        image_info = []
        image_info.append("filename: " + file_name_piece[-1] + "  ")
        image_info.append("format: " + str(image.format) + "  ")
        image_info.append("size: " + str(image.size))
        self.image_info_label.config(text="".join(image_info))


def attach_menubar(window):
    menu_bar = Menu(window)
    window.configure(menu=menu_bar)
    menu_file = Menu(menu_bar, tearoff=False)
    menu_edit = Menu(menu_bar, tearoff=False)
    menu_bar.add_cascade(label="File", underline=0, menu=menu_file)
    menu_bar.add_cascade(label="Edit", underline=0, menu=menu_edit)

if __name__ == "__main__":
    root_window = Tk()
    attach_menubar(root_window)
    photo_viewer_frame = PhotoViewerFrame(root_window)
    root_window.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
