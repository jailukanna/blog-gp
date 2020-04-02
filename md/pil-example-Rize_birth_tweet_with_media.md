---
title: pil example Rize birth tweet with media (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Rize birth tweet with media'

Functions in program: 
* `def tweet(day, api, image):`
* `def setup():`
* `def draw(path, day):`
* `def choose():`
* `def calc(today, birth_month, birth_date):`

Modules used in program: 
* `import tweepy`
* `import random`
* `import os`
* `import datetime`

## python Rize birth tweet with media

Python pil example: Rize birth tweet with media

```python
# -*- coding: utf-8 -*-

import datetime
import os
import random
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
import tweepy


def calc(today, birth_month, birth_date):
    birth_year = today.year
    birth_day = datetime.date(birth_year, birth_month, birth_date)
    calc = birth_day - today
    day = calc.days
    if day < 0:
        birth_year += 1
        birth_day = datetime.date(birth_year, birth_month, birth_date)
        calc = birth_day - today
        day = calc.days
    return day

def choose():
    get = os.listdir('C:\\Users\\Pictures\\Rize')　#写真が置いてあるフォルダのパスを入力
    image = random.choice(get)
    return image

def draw(path, day):
    while True:
        try:
            ba = Image.open(path).convert('RGBA')
            base = ba.resize((1200, 675))
            mark = 'リゼちゃんの誕生日まであと{}日'.format(day)
            fontsize = 60
            opacity = 300
            color = (204, 46, 250)
            txt = Image.new('RGBA', base.size, (255, 255, 255, 0))
            draw = ImageDraw.Draw(txt)
            fnt = ImageFont.truetype(font='C:\\Windows\\Fonts\\meiryo.ttc', size=fontsize) #フォントのパスを入力
            textw, texth = draw.textsize(mark, font=fnt)
            draw.text(((base.width - textw) / 2, (base.height - texth) / 4 * 3),
              mark, font=fnt, fill=color + (opacity,))
            out = Image.alpha_composite(base, txt)
            name = '{}_{}.png'.format(path, day)
            out.save(name, 'png', quality=100, optimize=True)
            print(name)
            break
        except:
            pass
    return name

def setup():
    CONSUMER_KEY = "xxxxxxxxxxxxxxxx"
    CONSUMER_SECRET = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
    ACCESS_TOKEN = "xxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ACCESS_TOKEN_SECRET = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    auth.set_access_token(ACCESS_TOKEN, ACCESS_SECRET)
    return auth

def tweet(day, api, image):
    twi = 'リゼちゃんの誕生日まであと{}日 #Rize_Birth_Counter'.format(day)
    api.update_with_media(image, twi)
    os.remove(image)


result = calc(datetime.date.today(), 2, 14)
image = choose()
create = draw(image, result)
api = setup()
tweet(result, api, create)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
