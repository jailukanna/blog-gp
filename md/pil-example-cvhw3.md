---
title: pil example cvhw3 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'cvhw3'

Functions in program: 
* `def __del__(self):`

Modules used in program: 
* `import numpy as np`
* `import PIL.Image, PIL.ImageTk`
* `import tkinter`
* `import time`
* `import uuid`
* `import cv2`

## python cvhw3

Python pil example: cvhw3

```python
from tkinter import *
from tkinter import ttk
from PIL import ImageTk,Image
from tkinter.filedialog import askopenfilename
import cv2
import uuid
import time
import tkinter
import PIL.Image, PIL.ImageTk
from PIL import Image
import numpy as np





class Assignment:

    def __init__(self,master):

        self.header =  ttk.Frame(master,relief='groove')

        self.header.pack()

        ttk.Label(self.header,text="Computer Vision Assignment HW3").grid(row=0, column=0)

        self.content = ttk.Frame(master)

        self.content.tk_setPalette(background='#40E0D0', foreground='black',
               activeBackground='black', activeForeground='#40E0D0')
        self.content.pack()


        ttk.Button(self.content,text='Open Image',command=self.openimg).grid(row=0,column=0)
        ttk.Button(self.content, text='Open Video',command=self.openvideo).grid(row=0, column=2)
        ttk.Button(self.content, text='Implement Blending',command=self.blend).grid(row=0, column=4)
        ttk.Button(self.content, text='Screen Shot ',command=self.screenshot).grid(row=0,column=6)
        ttk.Button(self.content, text='video capture',command=self.videocapture).grid(row=0, column=8)

        self.bottom = ttk.Frame(master)
        self.bottom.pack()

        self.bottomdown= ttk.Frame(master)
        self.bottomdown.pack()



    def openimg(self):

      self.filename = askopenfilename()
      img = ImageTk.PhotoImage(Image.open(self.filename))
      imgholder = ttk.Label(self.bottom,image=img).grid(row=0,column=0)
      imgholder.pack()

    def openvideo(self):
         self.filename = askopenfilename()
         video = cv2.VideoCapture(self.filename)
         while(video.isOpened()):
            ret,frame = video.read()

            cv2.imshow('frame',frame)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
         video.release()
         cv2.destroyAllWindows()

    def openwebcam(self):
        video = cv2.VideoCapture(0)
        while (video.isOpened()):
            ret, frame = video.read()

            cv2.imshow('frame', frame)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
        video.release()
        cv2.destroyAllWindows()

    def videocapture(self):
        video = cv2.VideoCapture(0)
        codec = cv2.VideoWriter_fourcc(*'DIVX')
        out = cv2.VideoWriter(str(uuid.uuid4())+'.avi',codec,20.0,(640,480))
        while(video.isOpened()):
            ret, frame = video.read()
            if ret == True:
                out.write(frame)
                cv2.imshow('frame', frame)
                if cv2.waitKey(1) & 0xFF == ord('q'):
                    break
            else:
                break



        video.release()
        video.destroyAllWindows()

    def blend(self):

        image1 = cv2.imread("apple.jpg")
        image1 = cv2.resize(image1, (200, 200))
        image2 = cv2.imread("orange.jpg")
        image2 = cv2.resize(image2, (200, 200))

        apple = np.hstack((image1[:, :100], image2[:, 100:]))


        layer = image1.copy()
        gaussian_pyr = [layer]
        for i in range(6):
            layer = cv2.pyrDown(layer)
            gaussian_pyr.append(layer)


        layer = gaussian_pyr[5]
        laplacian_pyr = [layer]
        for i in range(5, 0, -1):
            size = (gaussian_pyr[i - 1].shape[1], gaussian_pyr[i - 1].shape[0])
            gaussian_expanded = cv2.pyrUp(gaussian_pyr[i], dstsize=size)
            laplacian = cv2.subtract(gaussian_pyr[i - 1], gaussian_expanded)
            laplacian_pyr.append(laplacian)


        layer = image2.copy()
        gaussian_pyr2 = [layer]
        for i in range(6):
            layer = cv2.pyrDown(layer)
            gaussian_pyr2.append(layer)


        layer = gaussian_pyr2[5]
        laplacian_pyr2 = [layer]
        for i in range(5, 0, -1):
            size = (gaussian_pyr2[i - 1].shape[1], gaussian_pyr2[i - 1].shape[0])
            gaussian_expanded = cv2.pyrUp(gaussian_pyr2[i], dstsize=size)
            laplacian = cv2.subtract(gaussian_pyr2[i - 1], gaussian_expanded)
            laplacian_pyr2.append(laplacian)


        apple_pyramid = []
        n = 0
        for img1_lap, img2_lap in zip(laplacian_pyr, laplacian_pyr2):
            n += 1
            cols, rows, ch = img1_lap.shape
            laplacian = np.hstack((img1_lap[:, 0:int(cols / 2)], img2_lap[:, int(cols / 2):]))
            apple_pyramid.append(laplacian)


        apple_reconstructed = apple_pyramid[0]
        for i in range(1, 6):
            size = (apple_pyramid[i].shape[1], apple_pyramid[i].shape[0])
            apple_reconstructed = cv2.pyrUp(apple_reconstructed, dstsize=size)
            apple_reconstructed = cv2.add(apple_pyramid[i], apple_reconstructed)

        cv2.imshow("apple reconstructed", apple_reconstructed)
        cv2.imshow("apple", apple)
        cv2.imshow("img1", image1)
        cv2.imshow("img2", image2)
        cv2.waitKey(0)
        cv2.destroyAllWindows()



    def screenshot(self):

        self.vid = CapVideo(0)
        self.canvas = tkinter.Canvas(self.bottomdown, width=self.vid.width, height=self.vid.height)
        self.canvas.pack()
        self.btn_snapshot = tkinter.Button(self.bottomdown, text="Snapshot", width=50, command=self.snap)
        self.btn_snapshot.pack(anchor=tkinter.CENTER, expand=True)

        self.update()

    def snap(self):

         ret, frame = self.vid.Frame()

         if ret:
            cv2.imwrite(str(uuid) +".jpg", cv2.cvtColor(frame, cv2.COLOR_RGB2BGR))

    def update(self):
           ret, frame = self.vid.Frame()
           if ret:
             self.photo = PIL.ImageTk.PhotoImage(image = PIL.Image.fromarray(frame))
             self.canvas.create_image(0, 0, image = self.photo, anchor = tkinter.NW)
           self.bottomdown.after(20, self.update)

class CapVideo:
        def __init__(self, video_source=0):

            self.vid = cv2.VideoCapture(video_source)


            self.width = self.vid.get(cv2.CAP_PROP_FRAME_WIDTH)
            self.height = self.vid.get(cv2.CAP_PROP_FRAME_HEIGHT)

        def Frame(self):
            if self.vid.isOpened():
                ret, frame = self.vid.read()
                if ret:
                    return (ret, cv2.cvtColor(frame, cv2.COLOR_BGR2RGB))
                else:
                    return (ret, None)
            else:
                return 0


def __del__(self):
        if self.video.isOpened():
            self.video.release()
root = Tk()
root.title("CV HW:3")
assignment = Assignment(root)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
