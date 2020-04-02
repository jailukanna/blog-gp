---
title: pil example Pycharm project CaptchaImage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Pycharm project CaptchaImage'

Functions in program: 
* `def TestCaptchaImage():`
* `def GetCaptchaTextAndImage(charCount=4):`
* `def RandomCharSet(charCount=4):`

## python Pycharm project CaptchaImage

Python pil example: Pycharm project CaptchaImage

```python
def RandomCharSet(charCount=4):
    import Definitions
    import random as rand

    charChoices = []

    for i in range(charCount):
        charChoices.append(str(rand.choice(Definitions.charSet)))

    return charChoices




def GetCaptchaTextAndImage(charCount=4):
    from captcha.image import ImageCaptcha
    import numpy as np
    from PIL import Image

    captcha = ImageCaptcha()
    charSet = RandomCharSet(charCount)
    image = captcha.generate(charSet)
    image = Image.open(image)
    image = np.array(image)

    return charSet, image


def TestCaptchaImage():
    from PIL import Image
    from matplotlib.pyplot import imshow
    text, image = GetCaptchaTextAndImage(4)

    print(text)
    imshow(image)
    print(type(image))
    print(image.shape)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
