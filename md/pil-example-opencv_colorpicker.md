---
title: pil example opencv colorpicker (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'opencv colorpicker'


Modules used in program: 
* `import numpy as np`
* `import cv2`
* `import tkFileDialog`

## python opencv colorpicker

Python pil example: opencv colorpicker

```python
# coding: utf-8
# USAGE
# opencv_colorpicker.py

# import the necessary packages
from Tkinter import Tk
from Tkinter import *
from PIL import Image
from PIL import ImageTk
import tkFileDialog
import cv2
import numpy as np

class OpenCVColorPicker:
    def select_image(self):

        # open a file chooser dialog and allow the user to select an input
        # image
        self.path = tkFileDialog.askopenfilename()

        # ensure a file path was selected
        if len(self.path) > 0:
            # load the image from disk, convert it to grayscale, and detect
            # edges in it
            self.cv2_image = cv2.imread(self.path)
            self.root.geometry('%dx%d' % self.cv2_image.shape[-2::-1])
            self.attatch_filter()

    def create_window(self):
        # initialize the window toolkit along with the two image panels
        # root = Tk()

        self.controll_window = Toplevel()
        self.controll_window.title('controller')
        # create a button, then when pressed, will trigger a file chooser
        # dialog and allow the user to select an input image; then add the
        # button the GUI
        btn = Button(self.controll_window, text="Select an image", command=self.select_image)
        btn.pack(side="top", fill="both", expand="yes", padx="10", pady="10")

        btn = Button(self.controll_window, text="attatch filter", command=self.attatch_filter)
        btn.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")

        w = Scale(self.controll_window, label="Brightness min", from_=0, to=255, orient=HORIZONTAL, variable=self.v_min, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")
        w = Scale(self.controll_window, label="Brightness max", from_=0, to=255, orient=HORIZONTAL, variable=self.v_max, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")
        w = Scale(self.controll_window, label="Saturation min", from_=0, to=255, orient=HORIZONTAL, variable=self.s_min, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")
        w = Scale(self.controll_window, label="Saturation max", from_=0, to=255, orient=HORIZONTAL, variable=self.s_max, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")
        w = Scale(self.controll_window, label="Hue min", from_=0, to=179, orient=HORIZONTAL, variable=self.h_min, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")
        w = Scale(self.controll_window, label="Hue max", from_=0, to=179, orient=HORIZONTAL, variable=self.h_max, command=self.attatch_filter)
        w.pack(side="bottom", fill="both", expand="yes", padx="10", pady="10")

        self.frame = Frame(self.root)
        self.frame.pack(fill=BOTH, expand=YES)
        self.canvas = Canvas(self.frame)
        self.canvas_image = self.canvas.create_image(0, 0, anchor=NW, image=None)
        self.canvas.pack(fill=BOTH, expand=YES)
        self.canvas.bind('<Configure>', self.canvas_resize)

        self.root.update()
        self.controll_window.update()
        self.controll_window.minsize(width=320, height=self.controll_window.winfo_height())
        
    def canvas_resize(self, event):
        return

    def attatch_filter(self, val=None):
        if self.path == "":
            return

        if self.cv2_image is None:
            return

        # self.image = cv2.imread(self.path)

        self.mask = self.create_mask(self.cv2_image)

        _edged = cv2.bitwise_and(self.cv2_image, self.cv2_image, mask=self.mask)

        # OpenCV represents images in BGR order; however PIL represents
        # images in RGB order, so we need to swap the channels
        _image = cv2.cvtColor(self.cv2_image, cv2.COLOR_BGR2RGB)
        _edged = cv2.cvtColor(_edged, cv2.COLOR_BGR2RGB)

        # convert the images to PIL format...
        _image = Image.fromarray(_image)
        _edged = Image.fromarray(_edged)

        # ...and then to ImageTk format
        self.image = ImageTk.PhotoImage(_image)
        self.edged = ImageTk.PhotoImage(_edged)

        # if the panels are None, initialize them
        # if self.panelA is None:
        #     # the first panel will store our original image
        #     # self.panelA = Label(image=self.image)
        #     self.panelA = Label(image=self.edged)
        #     self.panelA.image = self.image
        #     self.panelA.pack(side="left", padx=10, pady=10)
        #
        #     # while the second panel will store the edge map
        #     # self.panelB = Label(image=self.edged)
        #     # self.panelB.image = self.edged
        #     # self.panelB.pack(side="right", padx=10, pady=10)
        #
        # # otherwise, update the image panels
        # else:
        #     # update the pannels
        #     if self.orgflag:
        #         self.panelA.configure(image=self.image)
        #         self.panelA.image = self.edged
        #     else:
        #         self.panelA.configure(image=self.edged)
        #         self.panelA.image = self.edged
        img = None
        if self.orgflag:
            img = self.image
        else:
            img = self.edged
        self.canvas.itemconfigure(self.canvas_image, image=img)
        # self.root.update()

    def create_mask(self, image):
        h_min_ = self.h_min.get()
        h_max_ = self.h_max.get()
        if (h_min_ <= h_max_):
            hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
            lower_red = np.array([h_min_, self.s_min.get(), self.v_min.get()])
            upper_red = np.array([h_max_, self.s_max.get(), self.v_max.get()])
            return cv2.inRange(hsv, lower_red, upper_red)

        # h_max_ > h_min_
        hsv = cv2.cvtColor(self.cv2_image, cv2.COLOR_BGR2HSV)
        lower_red = np.array([0, self.s_min.get(), self.v_min.get()])
        upper_red = np.array([h_max_, self.s_max.get(), self.v_max.get()])
        mask = cv2.inRange(hsv, lower_red, upper_red)
        lower_red = np.array([h_min_, self.s_min.get(), self.v_min.get()])
        upper_red = np.array([179, self.s_max.get(), self.v_max.get()])
        mask2 = cv2.inRange(hsv, lower_red, upper_red)
        return cv2.bitwise_or(mask, mask2)

    def keyup(self, e):
        if e.char == ' ':
            self.orgflag = not self.orgflag
            self.attatch_filter()

    def __init__(self):
        self.root = Tk()
        self.root.title('Main')
        self.root.bind("<KeyRelease>", self.keyup)
        self.path = ''
        self.controll_window = None
        self.cv2_image = None
        self.image = None
        self.mask = None
        # self.panelA = None
        self.frame = None
        self.canvas = None
        self.canvas_image = None
        self.h_max = IntVar()
        self.h_max.set(20)
        self.h_min = IntVar()
        self.h_min.set(160)
        self.s_max = IntVar()
        self.s_max.set(200)
        self.s_min = IntVar()
        self.s_min.set(0)
        self.v_max = IntVar()
        self.v_max.set(255)
        self.v_min = IntVar()
        self.v_min.set(200)

        self.orgflag = False
        self.create_window()
        self.select_image()
        # kick off the GUI
        self.root.mainloop()

obj = OpenCVColorPicker()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
