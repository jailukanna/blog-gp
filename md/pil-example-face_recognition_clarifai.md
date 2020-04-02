---
title: pil example face recognition clarifai (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'face recognition clarifai'


## python face recognition clarifai

Python pil example: face recognition clarifai

```python
from clarifai import rest
from clarifai.rest import ClarifaiApp
from clarifai.rest import Image as ClImage
from PIL import Image as PilImage

CLARIFAI_APP_ID = "some_long_string"
CLARIFAI_APP_SECRET = "some_long_string"

##Grab the image
the_image = 'path/to/image.jpg'
smiley = '/path/to/overlay.jpg'

app = ClarifaiApp(CLARIFAI_APP_ID, CLARIFAI_APP_SECRET)

##face recognition model
model = app.models.get('a403429f2ddf4b49b307e318f00e528b')
image = ClImage(filename=the_image)

data = model.predict([image])

##find the bounding box
box = data['outputs'][0]['data']['regions'][1]['region_info']['bounding_box']

back = PilImage.open(the_image)
back_w, back_h = back.size

fore = PilImage.open(smiley)

smiley_width = (box['right_col'] - box['left_col']) * back_w
smiley_height = (box['bottom_row'] - box['top_row']) * back_h

maxsize = (smiley_width, smiley_height)
fore.thumbnail(maxsize, PilImage.ANTIALIAS)

#fore.save('temp.png')

#temp = PilImage.open('temp.png')

back.paste(fore, (round(box['left_col'] * back_w), round(box['top_row'] * back_h)), fore)
back.save('out.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
