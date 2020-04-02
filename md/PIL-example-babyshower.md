---
title: PIL example babyshower (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'babyshower'

Functions in program: 
* `def get_font_size(width, height):`

Modules used in program: 
* `import random`
* `import glob, os`

## python babyshower

Python PIL example: babyshower

```python
#Plow through all images in the folder
#Write a tracking number in the top left corner
#Save a text file that shows the original filename and the 
#tracking number.
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw 
import glob, os
import random

index = 1
answers = {}

def get_font_size(width, height):
    return (height) / 15 #font size 24 for image height 1400

#Get all files and then shuffle them... alphabetic answers
#could be suspicious and predictable to astute guests
files = glob.glob("*.jpg")
random.shuffle(files)

for file in files:
    answers[index] = file

    img = Image.open(file)
    draw = ImageDraw.Draw(img)
    font_mult = get_font_size(img.width, img.height)
    font = ImageFont.truetype("arial.ttf", font_mult)
    
    draw.text((0, 0), "#" + str(index),(255,0,0),font=font)
    img.save("output/" + str(index) + ".jpg")
    
    index += 1
    
# write out the answers, sorted by key
with open('answers.txt', 'w') as file:
    keylist = answers.keys()
    keylist.sort()
    for k in keylist:
        file.write(str(k) + ": " + answers[k] + "\n")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
