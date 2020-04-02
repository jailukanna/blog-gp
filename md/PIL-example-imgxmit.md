---
title: PIL example imgxmit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'imgxmit'

Functions in program: 
* `def take_image(image_name):`
* `def overlay_callsign(image, callsign, timestamp):`
* `def main():`
* `def xmit(image):`
* `def quick_timestamp():`

Modules used in program: 
* `import picamera`
* `import subprocess`
* `import time`
* `import io`

## python imgxmit

Python PIL example: imgxmit

```python
#!/usr/bin/env python

"""
This script
* acquires Image
* overlays callsign on Image
* converts image to wav
* plays wav
"""


import io
import time
import subprocess
from os import remove

import picamera
from PIL import Image, ImageFont, ImageDraw
from pysstv.color import MartinM1


def quick_timestamp():
    return time.time()


def xmit(image):
    print("Encoding image to wav file: %s" % quick_timestamp())
    sstv = MartinM1(image, 48000, 16)
    print("Writing wav file: %s" % quick_timestamp())
    sstv.write_wav("out.wav")
    print("Playing wav file: %s" % quick_timestamp())
    wav_filename = "out.wav"
    subprocess.check_call(["aplay", wav_filename])
    remove(wav_filename)


def main():
    timestamp = quick_timestamp()
    image_name = "images/image-%s.jpg" % timestamp

    img = take_image(image_name)

    img = overlay_callsign(img.rotate(180), "W7ASU", timestamp)
    img.save(image_name)
    xmit(img)
    print("imgxmit complete %s" % quick_timestamp())
    print("runtime: %2.5s (s)" % str(time.time() - timestamp))


def overlay_callsign(image, callsign, timestamp):
    """Given a PIL Image Object will overlay the club callsign on the image
    and return the modified image object
    """
    print("Overlaying callsign: %s" % quick_timestamp())
    font = ImageFont.truetype(
        "/usr/share/fonts/truetype/roboto/Roboto-Bold.ttf",
        24
    )
    draw = ImageDraw.Draw(image)
    text = "%s : %s" % (callsign, timestamp)
    draw.text((20, 220), text, (255, 255, 255), font=font)
    return image


def take_image(image_name):
    """Takes image and returns PIL Image object"""
    print("Taking image %s" % image_name)

    stream = io.BytesIO()
    with picamera.PiCamera() as camera:
        camera.resolution = (320, 256)
        camera.start_preview()

        time.sleep(2)

        camera.shutter_speed = camera.exposure_speed
        camera.exposure_mode = 'off'
        g = camera.awb_gains
        camera.awb_mode = 'off'
        camera.awb_gains = g

        camera.capture(stream, format='jpeg')

    # "Rewind" the stream to the beginning so we can read its content
    stream.seek(0)
    return Image.open(stream)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
