---
title: PIL example mp3-player (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'mp3-player'

Functions in program: 
* `def GetTag(songfile):`
* `def WrapText(mx, my, mtxt):`
* `def ResetView():`
* `def play_mp3(path):`

Modules used in program: 
* `import subprocess`
* `import Adafruit_SSD1306`
* `import os.path`
* `import eyed3`
* `import sys`
* `import time`

## python mp3-player

Python PIL example: mp3-player

```python
import time
import sys
import eyed3
import os.path

import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

import subprocess

def play_mp3(path):
    subprocess.Popen(['mpg123', '-q', path]).wait()

def ResetView():
    global x
    global top
    x = 0
    top = -2
    disp.clear()
    disp.display()
    draw.rectangle((0, 0, width, height), outline=0, fill=0)
    disp.image(image)
    disp.display()

def WrapText(mx, my, mtxt):
    if len(mtxt) >= 20:
        while len(mtxt) >= 20:
            a = mtxt[0:20]
            draw.text((mx, my), str(a),  font=font, fill=255)
            disp.image(image)
            disp.display()
            b = mtxt[21:len(mtxt)]
            mtxt = b
            mx = 0
            my += 8
        draw.text((mx, my), str(mtxt),  font=font, fill=255)
        disp.image(image)
        disp.display()
    else:
        draw.text((mx, my), str(mtxt),  font=font, fill=255)
        disp.image(image)
        disp.display()

def GetTag(songfile):
    global top
    audiofile = eyed3.load(songfile)
    artist = audiofile.tag.artist
    album = audiofile.tag.album
    title = audiofile.tag.title
    tmptrack_num = audiofile.tag.track_num
    track_num_a = tmptrack_num[0]
    track_num_b = tmptrack_num[1]
    if track_num_a:
        if track_num_b:
            track_num = str(track_num_a) + '/' + str(track_num_b)
        else:
            track_num = str(track_num_a)
    else:
        track_num = ''
    ResetView()
    draw.text((x, top), str(title), font=font, fill=255)
    draw.text((x, top+8), str(artist), font=font, fill=255)
    draw.text((x, top+16), str(album), font=font, fill=255)
    draw.text((x, top+24), str(track_num), font=font, fill=255)
    disp.image(image)
    disp.display()
    play_mp3(songfile)


RST = None
disp = Adafruit_SSD1306.SSD1306_128_64(rst=RST)
disp.begin()
disp.clear()
disp.display()

width = disp.width
height = disp.height
image = Image.new('1', (width, height))
draw = ImageDraw.Draw(image)
draw.rectangle((0,0,width,height), outline=0, fill=0)
padding = -2
top = padding
bottom = height-padding
x = 0

font = ImageFont.load_default()

agv_num = len(sys.argv)

if agv_num == 1:
    sys.exit("usage: " + sys.argv[0] + " songs")
draw.rectangle((0, 0, width, height), outline=0, fill=0)

song_list = []

for i in range(1, agv_num):
    tmpthing = sys.argv[i]
    if os.path.isfile(tmpthing):
        song_list.append(tmpthing)
    else:
        if os.path.isdir(tmpthing):
            for subdir, dirs, files in os.walk(tmpthing):
                for file in files:
                    tmptwo = os.path.join(subdir, file)
                    song_list.append(tmptwo)
for i in range(0, len(song_list)):
    song = song_list[i]
    GetTag(song)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
