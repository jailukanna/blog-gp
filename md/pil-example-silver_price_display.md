---
title: pil example silver price display (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'silver price display'

Functions in program: 
* `def main():`
* `def save_image():`
* `def get_silver_price():`

Modules used in program: 
* `import requests`
* `import time`

## python silver price display

Python pil example: silver price display

```python
import time
from subprocess import Popen

from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import requests
from bs4 import BeautifulSoup

URL = 'http://www.silverpriceoz.com/silver-price-per-ounce/'


def get_silver_price():
    r = requests.get(URL)
    soup = BeautifulSoup(r.content)
    price_text = soup.find_all('b')[2].text.lstrip()
    return price_text[0:5]


def save_image():
    price = get_silver_price()
    text = (("Price per ounce $", (0, 255, 0)), (price, (192, 192, 192)))

    font = ImageFont.truetype("/usr/share/fonts/TTF/FreeSans.ttf", 16)
    all_text = ""
    for text_color_pair in text:
        t = text_color_pair[0]
        all_text = all_text + t

    print(all_text)
    width, ignore = font.getsize(all_text)
    print(width)

    im = Image.new("RGB", (width + 30, 16), "black")
    draw = ImageDraw.Draw(im)

    x = 0;
    for text_color_pair in text:
        t = text_color_pair[0]
        c = text_color_pair[1]
        print("t=" + t + " " + str(c) + " " + str(x))
        draw.text((x, 0), t, c, font=font)
        x = x + font.getsize(t)[0]

    im.save("test.ppm")


def main():
    while True:
        save_image()
        p = Popen(['./led-matrix', '1', 'test.ppm'])
        time.sleep(600) # 10 minutes
        p.terminate()
        p.wait()


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
