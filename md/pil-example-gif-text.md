---
title: pil example gif-text (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'gif-text'


Modules used in program: 
* `import shutil`
* `import os`

## python gif-text

Python pil example: gif-text

```python
# OS modules
import os
import shutil

# Image modules
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

# create/delete our temp files folder
if os.path.exists('frames'):
    shutil.rmtree('frames')
os.mkdir('frames')

# load in the background image
img_background = Image.open('background.png')
img = Image.new("RGBA", img_background.size, (0, 0, 0, 255))

# set up colours and vars
x = 10
y = 10
silver = (100, 100, 100, 255)
purple = (100, 0, 200, 255)
white = (255, 255, 255, 255)
text = '''Boing!'''

# load the font
font = ImageFont.truetype("/home/chrisg/Ubuntu-B.ttf", 75)
draw = ImageDraw.Draw(img)

# add text to each frame
for N in range(0, 24):
    y += N
    img.paste(img_background, (0, 0))
    draw.text((x+4, y+4), text, purple, font=font)
    draw.text((x+2, y+2), text, silver, font=font)
    draw.text((x, y), text, white, font=font)
    img.save("./frames/{}.png".format(str(N).zfill(3)))

# output the result
os.system('ffmpeg -framerate 24 -i frames/%03d.png -c:v ffv1 -r 24 -y out.avi')
os.system('ffmpeg -y -i out.avi out.gif')

# clean up
shutil.rmtree('frames')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
