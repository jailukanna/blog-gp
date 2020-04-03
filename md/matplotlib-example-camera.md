---
title: matplotlib example camera (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'camera'

Functions in program: 
* `def updatefig(*args):`

Modules used in program: 
* `import matplotlib.cm as cm`
* `import matplotlib.pyplot as plt`
* `import matplotlib.animation as animation`
* `import time`
* `import cv2`
* `import numpy as np`

## python camera

Python matplotlib example: camera

```python
# Display webcam image, plus plasma diagnostics
# version 2, 2013-09-25
# Amar
# Changelog:
#       v2: Very slight modification of http://matplotlib.org/examples/animation/dynamic_image.html

import numpy as np
import cv2
import time
import matplotlib.animation as animation
import matplotlib.pyplot as plt
import matplotlib.cm as cm

try: import Tkinter as Tk
except ImportError: import tkinter as Tk
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.figure import Figure

class Camera:
    def __init__(self,channel=0):
        self.capture = cv2.VideoCapture(channel)
        self.success,self.image = self.capture.read()
        print("Beginning acquisition ...",)

    def acquire(self):
        self.success,self.image = self.capture.read()

    def close(self):
        if self.capture.isOpened(): self.capture.release()
        print("released camera")

camera = Camera()
fig = plt.figure()
fig.canvas.set_window_title('USB Camera') 
im = plt.imshow(camera.image[:,:,0])

def updatefig(*args):
    camera.acquire()
    im.set_array(camera.image[:,:,0])
    return im,

ani = animation.FuncAnimation(fig, updatefig, interval=80, blit=True)
plt.show()
camera.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
