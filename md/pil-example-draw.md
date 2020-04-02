---
title: pil example draw (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'draw'

Functions in program: 
* `def paint_pixel(args):`
* `def similar_color(p1, p2):`
* `def to_rgb(pixel):`
* `def from_rgb(r, g, b):`
* `def get_board():`
* `def get_image_and_resize(file, baseheight):`
* `def paint(user_token, x, y):`
* `def update_color(user_token, color):`
* `def login(username):`

Modules used in program: 
* `import time`
* `import os`
* `import marshal`
* `import PIL`
* `import requests`

## python draw

Python pil example: draw

```python
# https://github.com/nytr0gen

import requests
import PIL
import marshal
import os
import time
from PIL import Image
from multiprocessing import Pool

session = requests.Session()

api_url = 'http://167.172.165.153:60001'
hacktm_clean = "hacktm"
hacktm = "hac\u212atm"

assert(hacktm.lower() == hacktm_clean.lower())
assert(hacktm.upper() != hacktm_clean.upper())

def login(username):
    global session

    payload = {'username': username}
    r = session.post(api_url + '/login', json=payload)
    data = r.json()

    return data # data["data"]["token"]

def update_color(user_token, color):
    global session

    payload = {'color': color}
    headers = {
        'Authorization': 'Bearer ' + user_token,
    }
    r = session.post(api_url + '/updateUser', headers=headers, json=payload)
    data = r.json()

    return data

def paint(user_token, x, y):
    global session

    payload = {'x': x, 'y': y}
    headers = {
        'Authorization': 'Bearer ' + user_token,
    }

    try:
        session.post(api_url + '/paint', headers=headers, json=payload)
    except Exception:
        pass

def get_image_and_resize(file, baseheight):
    img = Image.open(file)
    hpercent = (baseheight / float(img.size[1]))
    wsize = int((float(img.size[0]) * float(hpercent)))
    img = img.resize((wsize, baseheight), PIL.Image.ANTIALIAS)
    return img

def get_board():
    global session

    r = session.get(api_url + '/')
    data = r.json()
    board = data['data']['board']
    for y, row in enumerate(board):
        board[y] = [int(v) for v in row.strip().split(',')]

    return board

def from_rgb(r, g, b):
    return (r << 16) + (g << 8) + b

def to_rgb(pixel):
    r = (pixel >> 16) & 255
    g = (pixel >> 8) & 255
    b = pixel & 255
    return r, g, b

def similar_color(p1, p2):
    r1, g1, b1 = to_rgb(p1)
    r2, g2, b2 = to_rgb(p2)

    if abs(r1 - r2) >= 0x40:
        return False
    if abs(g1 - g2) >= 0x40:
        return False
    if abs(b1 - b2) >= 0x40:
        return False
    return True

# data = login(hacktm)
# token = data["data"]["token"]
# print(token)
# update_color(token, 0x070506)
# paint(token, 0, 0)
# raise ValueError(1)

marshal_file = "./color_token.marshal"
color_token = {}
# if os.path.exists(marshal_file):
#     with open(marshal_file, "rb") as f:
#         color_token = marshal.load(f)
#         print("Loaded some stuff from file %d" % len(color_token))

def paint_pixel(args):
    color_token, x, y, color = args
    if color not in color_token:
        data = login(hacktm)
        token = data["data"]["token"]
        update_color(token, color)
        color_token[color] = token
    else:
        token = color_token[color]

    try:
        paint(token, x, y)
    except Exception:
        pass

baseheight = 30
img = get_image_and_resize('1280px-Flag_of_Romania.png', baseheight)
pixels = img.load()
width, height = img.size

p = Pool(20)

while True:
    board = get_board()
    base_x = 0
    base_y = 40

    tasks = []
    for x in range(width):
        for y in range(height):
            px = pixels[x, y]

            r = (px[0] // 0x20) * 0x20
            g = (px[1] // 0x20) * 0x20
            b = (px[2] // 0x20) * 0x20

            if len(px) == 4 and px[3] < 64:
                continue

            color = from_rgb(r, g, b)

            bx = x+base_x
            by = y+base_y
            if not similar_color(color, board[by][bx]):
                tasks.append((color_token, bx, by, color))

    print("We have some tasks %d" % len(tasks))
    p.map(paint_pixel, tasks)
    time.sleep(0.6)

p.terminate()

# print(color_token)
# with open(marshal_file, "wb") as f:
#     marshal.dump(color_token, f)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
