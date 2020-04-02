---
title: pil example Tibetan Image Wylie to druktsa (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Tibetan Image Wylie to druktsa'


Modules used in program: 
* `import numpy as np`

## python Tibetan Image Wylie to druktsa

Python pil example: Tibetan Image Wylie to druktsa

```python
from PIL import Image, ImageDraw, ImageFont
from matplotlib.pyplot import imshow
import numpy as np
from PIL import Image
%matplotlib inline


img = Image.new('RGB', (1000, 1000), color = (73, 109, 137))
 
#location of font files. 
#Convert uchen to ume image in tibetan script. 
#https://medium.com/@tenpa/learning-to-type-in-tibetan-installing-monlam-font-on-mac-os-x-caecd039c5f2
#put the tff file location. 

fnt = ImageFont.truetype('/Library/Fonts/TibetanSambhotaYigchung.ttf', 20)
d = ImageDraw.Draw(img)
d.text((10,10), '''༈ འཛམ་གླིང་མཛེས་པའི་རྒྱན་དྲུག་མཆོག་གཉིས་དང༌། །

ཐུགས་རྗེ་ལུང་རྟོགས་མཉམ་པའི་མཐུ་མངའ་ཡང༌། །

ནགས་ཁྲོད་དམ་པར་སྦས་པའི་བརྟུལ་ཞུགས་ཀྱིས། །

འཁོར་འདས་ཆོས་སྐུར་རྫོགས་པའི་ཀློང་ཆེན་པ། །

དྲི་མེད་འོད་ཟེར་ཞབས་ལ་གསོལ་བ་འདེབས། །''', font=fnt, fill=(255, 255, 0))
img.save('pil_text_font.png')

pil_im = Image.open('pil_text_font.png', 'r')
imshow(np.asarray(pil_im))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
