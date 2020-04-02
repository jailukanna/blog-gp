---
title: pil example certificate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'certificate'


Modules used in program: 
* `import requests`

## python certificate

Python pil example: certificate

```python
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
import requests
from io import BytesIO

#Read image
response = requests.get('https://www.hloom.com/images/Generic-Certificate-of-Completion.jpg')
img = Image.open(BytesIO(response.content))

draw = ImageDraw.Draw(img)

#select font type
selectFont = ImageFont.truetype("arial.ttf", 15)

#add cordinate and body and colour of the text

draw.text( (100,100), 'text', (0,0,0), font=selectFont)

#save to pdf
img.save('certi.pdf', "PDF", resolution=100.0)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
