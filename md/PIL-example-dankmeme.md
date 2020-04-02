---
title: PIL example dankmeme (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'dankmeme'

Functions in program: 
* `def creatememe(imgloc, fontloc, upzero, botzero, outloc):`

Modules used in program: 
* `import PIL`

## python dankmeme

Python PIL example: dankmeme

```python
#!/usr/bin/env python
# coding: utf-8

# In[1]:


import PIL
from PIL import ImageDraw, ImageFont
from PIL import Image
from PIL import ImageDraw


# In[7]:


def creatememe(imgloc, fontloc, upzero, botzero, outloc):

    money = Image.open(imgloc)
    #lisgal = os.listdir('/mnt/c/Users/luke/reducehrsavedol/galleries/')


    imageSize = money.size

    # find biggest font size that works
    fontSize = int(imageSize[1]/5)
    font = ImageFont.truetype(fontloc, fontSize)
    topTextSize = font.getsize(upzero)
    bottomTextSize = font.getsize(botzero)

    while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
        fontSize = fontSize - 1
        font = ImageFont.truetype(fontloc, fontSize)
        topTextSize = font.getsize(upzero)
        bottomTextSize = font.getsize(botzero)

    # find top centered position for top text
    topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
    topTextPositionY = 0
    topTextPosition = (topTextPositionX, topTextPositionY)

    # find bottom centered position for bottom text
    bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(money)

    outlineRange = int(fontSize/15)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), upzero, (0,0,0), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), botzero, (0,0,0), font=font)

        draw.text(topTextPosition, upzero, (255,255,255), font=font)
        draw.text(bottomTextPosition, botzero, (255,255,255), font=font)

        money.save(outloc)



# In[14]:





# In[ ]:






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
