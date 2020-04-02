---
title: PIL example luts (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'luts'


## python luts

Python PIL example: luts

```python

from omero.gateway import BlitzGateway
from PIL import Image

conn = BlitzGateway("user-3", "ome", host="eel.openmicroscopy.org", port=4064)
# Image is a 'fake' bio-formats image (grey intensity gradient)
imageId = 25501

conn.connect()

# luts = ["16_colors.lut",
#         "3-3-2_rgb.lut",
#         "5_ramps.lut",
#         "6_shades.lut",
#         "blue_orange_icb.lut",
#         "brgbcmyw.lut",
#         "cool.lut",
#         "cyan_hot.lut",
#         "edges.lut",
#         "fire.lut",
#         "gem.lut",
#         "grays.lut",
#         "green_fire_blue.lut",
#         "hilo.lut",
#         "ica.lut",
#         "ica2.lut",
#         "ica3.lut",
#         "ice.lut",
#         "magenta_hot.lut",
#         "orange_hot.lut",
#         "phase.lut",
#         "rainbow_rgb.lut",
#         "red-green.lut",
#         "red_hot.lut",
#         "royal.lut",
#         "sepia.lut",
#         "smart.lut",
#         "spectrum.lut",
#         "thal.lut",
#         "thallium.lut",
#         "unionjack.lut",
#         "yellow_hot.lut"]

scriptService = conn.getScriptService()
luts = scriptService.getScriptsByMimetype("text/x-lut")
lutNames = [l.name.val for l in luts]
lutNames.sort(key=lambda x: x.lower())

print(lutNames)

image = conn.getObject("Image", imageId)
image._prepareRenderingEngine()
image.setColorRenderingModel()

lutH = 10
h = len(luts) * lutH
canvas = Image.new("RGB", (256, h), (255, 255, 255))

for y, lut in enumerate(lutNames):
    image._re.setChannelLookupTable(0, lut)
    pilImg = image.renderImage(0, 0)
    c = pilImg.crop((0, 56 - lutH, 256, 56))
    canvas.paste(c, (0, y*lutH))
canvas.save('luts_%s.png' % lutH)
canvas.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
