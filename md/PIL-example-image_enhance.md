---
title: PIL example image enhance (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image enhance'


Modules used in program: 
* `import PIL.Image`

## python image enhance

Python PIL example: image enhance

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import PIL.Image
from PIL import ImageEnhance

IMAGE_PATH = "image.png"
SATURATION = 0.5
CONTRAST = 0.5
BRIGHTNESS = 0.5
SHARPNESS = 2.0

img = PIL.Image.open(IMAGE_PATH)

# 彩度を変える
saturation_converter = ImageEnhance.Color(img)
saturation_img = saturation_converter.enhance(SATURATION)

# コントラストを変える
contrast_converter = ImageEnhance.Contrast(img)
contrast_img = contrast_converter.enhance(CONTRAST)

# 明度を変える
brightness_converter = ImageEnhance.Brightness(img)
brightness_img = brightness_converter.enhance(BRIGHTNESS)


# シャープネスを変える
sharpness_converter = ImageEnhance.Sharpness(img)
sharpness_img = sharpness_converter.enhance(SHARPNESS)

#　画像を保存
saturation_img.save("saturation_out.png")
contrast_img.save("contrast_out.png")
brightness_img.save("brightness_out.png")
sharpness_img.save("sharpness_out.png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
