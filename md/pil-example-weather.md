---
title: pil example weather (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'weather'


Modules used in program: 
* `import numpy as np`
* `import time`
* `import PIL.ImageOps`

## python weather

Python pil example: weather

```python
#!/usr/bin/env python

# use selenium to take webpage screen shot based on stackoverflow from Corey Goldberg
# http://stackoverflow.com/questions/3422262/take-a-screenshot-with-selenium-webdriver/6282203#6282203
# http://stackoverflow.com/questions/3422262/take-a-screenshot-with-selenium-webdriver

from selenium import webdriver
from PIL import Image
import PIL.ImageOps
import time
import numpy as np

fox = webdriver.Firefox()
fox.get('http://mygeekdaddy.net/weather_mn.html')

time.sleep(5)

element = fox.find_element_by_id('weather')
location = element.location
size = element.size
fox.save_screenshot('weather_mn.png')
fox.quit()

# trim selenium screen shot inspired from stackoverlow
# http://stackoverflow.com/questions/15018372/how-to-take-partial-screenshot-with-selenium-webdriver-in-python

im = Image.open('weather_mn.png')

left = location['x']
top = location['y']
# dimensions added to capture just widget area
right = location['x'] + 500
bottom = location['y'] + 245

im = im.crop((left, top, right, bottom)) # defines crop points

im = im.convert("RGB")
im = PIL.ImageOps.invert(im)

im = im.convert("RGBA")
datas = im.getdata()


# make portions of image transparent inspired from stackoverflow by cr333
# http://stackoverflow.com/questions/765736/using-pil-to-make-all-white-pixels-transparent

newData = []
for item in datas:

    if item[0] == 0 and item[1] == 0 and item[2] == 0:
        newData.append((255, 255, 255, 0))
    else:
        newData.append(item)

im.putdata(newData)

# replace colors based on stackoverflow from Joe Kington
# http://stackoverflow.com/questions/3752476/python-pil-replace-a-single-rgba-color

data = np.array(im)   # "data" is a height x width x 4 numpy array
red, green, blue, alpha = data.T # Temporarily unpack the bands for readability

white_areas = (red == 0) & (blue == 0) & (green == 0)
data[..., :-1][white_areas] = (255, 255, 255)

im2 = Image.fromarray(data)

im2.save("weather_mn_desktop.png", "PNG")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
