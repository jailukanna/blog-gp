---
title: pil example qrscan (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'qrscan'

Functions in program: 
* `def read_codes(input):`

Modules used in program: 
* `import pygame.camera`
* `import pygame`
* `import pyzbar.pyzbar as pyzbar`
* `import PIL`

## python qrscan

Python pil example: qrscan

```python
import PIL
from PIL import Image
import pyzbar.pyzbar as pyzbar
import pygame
import pygame.camera
from pygame.locals import *

# set up the camera
pygame.init()
pygame.camera.init()

# clock so the loop does not freeze the pi
clock = pygame.time.Clock()

# set up our viewing screen
screen = pygame.display.set_mode((1280, 720))


# function that pulls data from the image
def read_codes(input):
    # read the QR code(s)
    codes = pyzbar.decode(input)

    # Print results
    for this_code in codes:
        if "QRCODE" == this_code.type:
            print('DETECTED: {}'.format(str(this_code.data)))

    return codes


# main function
if __name__ == '__main__':

    # use the first camera in the list
    camlist = pygame.camera.list_cameras()
    DEVICE = camlist[0]

    # reasonably high res capture, in RGB mode
    SIZE = (1280, 720)
    camera = pygame.camera.Camera(DEVICE, SIZE, "RGB")

    # start capture
    camera.start()

    while True:

        # get the image and display it at 0,0
        image = camera.get_image()
        pygame.image.save(image, "input.jpg")
        screen.blit(image, (0, 0))
        pygame.display.update()

        # read the image
        input_image = PIL.Image.open("input.jpg")

        # check the image
        result = read_codes(input_image)

        # 60 frames per second - essentially real time
        clock.tick(60)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
