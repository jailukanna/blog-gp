---
title: PIL example everyday-flavor (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'everyday-flavor'

Functions in program: 
* `def update_flavor(): `
* `def color_details(hex):`

Modules used in program: 
* `import PIL`
* `import tweepy`
* `import requests`
* `import random`
* `import os`

## python everyday-flavor

Python PIL example: everyday-flavor

```python
import os
import random
import requests
import tweepy
import PIL
from PIL import Image
from PIL import ImageColor
from PIL import ImageFile

# keys
consumer_key = ''
consumer_secret = ''
access_token = ''
access_token_secret = ''

def color_details(hex):
	r = requests.get('http://www.thecolorapi.com/id?hex=' + str(hex))
	return r.json()

def update_flavor(): 
	hue = int(360 * random.uniform(0, 1))
	saturation = int(50 + 30 * random.uniform(0, 1))
	luminance = int(60 + 35 * random.uniform(0, 1))

	colorHSL = 'hsl(%d, %d%%, %d%%)' % (hue, saturation, luminance)
	colorRGB = PIL.ImageColor.getrgb(colorHSL)
	colorHEX = '%02x%02x%02x' % colorRGB
	colorName = color_details(colorHEX)['name']['value']
	
	image = PIL.Image.new('RGB', (400, 400), colorRGB)
	image.save('image.jpg')

	path = os.path.abspath('image.jpg')

	api.update_profile_banner(path)
	api.update_profile_image(path)
	api.update_profile(name=colorName.lower(), profile_link_color='#' + colorHEX)

# init
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)

api = tweepy.API(auth)

update_flavor()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
