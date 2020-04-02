---
title: pil example app (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'app'

Functions in program: 
* `def form_example():`

Modules used in program: 
* `import subprocess`
* `import time`
* `import configparser`
* `import getpass`
* `import arrow`
* `import os`
* `import glob`
* `import arrow`
* `import tweepy`
* `import json`
* `import PIL`
* `import getpass`
* `import os`
* `import shutil`
* `import requests`

## python app

Python pil example: app

```python
from flask import Flask, request #import main Flask class and request object
import requests
import shutil
import os
import getpass
from urllib.parse import urlparse
import PIL
import json
from PIL import ImageDraw, ImageFont
from PIL import Image
from PIL import ImageDraw
from flask_bootstrap import Bootstrap
import tweepy
from flask import jsonify
import arrow
import glob

import os
import arrow
import getpass
import configparser
import time
import subprocess

myusr = getpass.getuser()
app = Flask(__name__) #create the Flask app
Bootstrap(app)

@app.route('/whatservice', methods=['GET', 'POST']) #allow both GET and POST requests
def form_example():
    if request.method == 'POST':  #this block is only entered when the form is submitted


        watservice = request.form.get('watservice') #if key doesn't exist, returns None
        toptext = 'slaps infoman' #if key doesn't exist, returns None
        bottext = 'this can fit so many \n{}'.format(watservice) #if key doesn't exist, returns a 400, bad request error

        img = Image.open('/home/pi/carmeme/slapinfo.jpg')

        imageSize = img.size

                # find biggest font size that works
        fontSize = int(imageSize[1]/5)
        font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
        topTextSize = font.getsize(toptext)
        bottomTextSize = font.getsize(bottext)

        while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
            fontSize = fontSize - 1
            font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
            topTextSize = font.getsize(toptext)
            bottomTextSize = font.getsize(bottext)

                # find top centered position for top text
        topTextPositionX = (imageSize[0]/4) - (topTextSize[0]/2)
        topTextPositionY = 0
        topTextPosition = (topTextPositionX, topTextPositionY)

                # find bottom centered position for bottom text
        bottomTextPositionX = (imageSize[0]/2)x - (bottomTextSize[0]/2)
        bottomTextPositionY = imageSize[1] - bottomTextSize[1] -100
        bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

        draw = ImageDraw.Draw(img)

        outlineRange = int(fontSize/15)
        for x in range(-outlineRange, outlineRange+1):
            for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (0,0,0), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottext, (0,0,0), font=font)

            draw.text(topTextPosition, toptext, (255,255,255), font=font)
            draw.text(bottomTextPosition, bottext, (255,255,255), font=font)

            img.save('/home/pi/carmeme/infomeme.jpg')
    #print(pngspli)


        return '''
                <p>Dog Meme Generator!</p>
                  <h1>{}
                  <br>{}</h1>
                  <br><a href="http://rbnz.tech:5123/galleries/images/">Link to gallery</a>

                  '''.format(toptext, bottext)

    return '''<!DOCTYPE html>
<html>

<head>

  <title>What Service Slap</title>
  <script src="https://unpkg.com/vue"></script>
  <script src="node_modules/vue/vue.min.js"></script>
  <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap/dist/css/bootstrap.min.css"/>
  <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap-vue@latest/dist/bootstrap-vue.css"/>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  </head>
  <body>
    <p>What Service Slap</p>
            <form method="POST">
                  what service: <input type="text" name="watservice"><br>
                  <input type="submit" value="Submit"><br>
              </form></body>'''


if __name__ == '__main__':
    app.run(host='0.0.0.0', debug=True, port=5000) #run app in debug mode on port 5000


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
