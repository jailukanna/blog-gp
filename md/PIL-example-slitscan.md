---
title: PIL example slitscan (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'slitscan'

Functions in program: 
* `def image_at_scale(scale):`

## python slitscan

Python PIL example: slitscan

```python
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

im = Image.open("scramble.png")
img_width, img_height = im.size
font = ImageFont.truetype("./Vera.ttf", 16)

def image_at_scale(scale):
    """ Apply slitscan effect for a given scale factor """
    data = im.copy().load()
    newpixels = []
    for x in range(img_width):
        for y in range(img_height):
            # Core slitscan algo
            newpixels.append(data[int(x+scale*y) % img_width,y])

    # Create a new image for these pixels
    im2 = Image.new(im.mode, im.size)
    im2.putdata(newpixels)

    # Create drawing context to burn in scale factor
    draw = ImageDraw.Draw(im2)
    draw.text((0, 0), "scale=%s" % scale,(255,255,255),font=font)

    return im2

if __name__ == "__main__":

    # Make Animated GIF for a range of scales...
    #output = []
    #for s in range(-200,201,20):
    #    scale = s/100.0
    #    print(scale)
    #    output.append(image_at_scale(scale))
    #
    #output[0].save('anitest.gif',
    #                save_all=True,
    #                append_images=output[1:],
    #                duration=100, # futz with this to change animation speed
    #                loop=0)

    # Save out a specfic scale
    image_at_scale(1.2).save("result.gif")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
