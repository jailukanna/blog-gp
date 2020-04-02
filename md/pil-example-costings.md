---
title: pil example costings (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'costings'

Functions in program: 
* `def savdols():`
* `def costsave(amemploy, avgpay):`

Modules used in program: 
* `import arrow`
* `import json`
* `import getpass`
* `import datetime`
* `import shutil`
* `import arrow`
* `import requests `
* `import locale`
* `import imageio`
* `import PIL`
* `import os`
* `import shutil`
* `import getpass`
* `import requests`
* `import json`

## python costings

Python pil example: costings

```python
from flask import Flask, request, jsonify
import json
import requests
import getpass
import shutil
import os
import PIL
import imageio
import locale

import requests 
import arrow
import shutil
from PIL import ImageFilter
import datetime
#import nltk
import getpass
import json
#import PIL
from PIL import ImageDraw, ImageFont, Image, ImageFilter
#from IPython.display import Image as disimg
from flask import send_file
from DiscordHooks import Hook, Embed, EmbedAuthor, Color
from datetime import datetime
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

myusr = getpass.getuser()
homepath = '/home/{}'.format(myusr)
app = Flask(__name__)
from urllib.parse import urlparse

import arrow

locale.setlocale( locale.LC_ALL, '' )

def costsave(amemploy, avgpay):
    return({'costsave' : locale.currency(int(amemploy) * 9 * 0.047 * int(avgpay) + int(amemploy) * 0.18 * 15000 * 0.20),
            'amountemployers' : amemploy, 'averagepage' : locale.currency(avgpay)})


@app.route('/savdols', methods=['GET', 'POST'])
def savdols():
        if request.method == 'POST':  #this block is only entered when the form is submitted
            ameploy = request.form.get('ameploy')
            avgpay = request.form.get('avgpay')
            #<input type="radio" name="results" value="10" checked> 10<br>

            costsaving = locale.currency(int(ameploy) * 9 * 0.047 * int(avgpay) + int(ameploy) * 0.18 * 15000 * 0.20)

            upzero = 'EMPLOY ' + str(ameploy) + ', PAY AVG ' + str(avgpay)
            botzero = 'SAVE ' + str(costsaving)

            money = Image.open('/mnt/c/Users/luke/Downloads/moneymoney.png')
            lisgal = os.listdir('/mnt/c/Users/luke/reducehrsavedol/galleries/')

                        
            imageSize = money.size

            # find biggest font size that works
            fontSize = int(imageSize[1]/5)
            font = ImageFont.truetype("/mnt/c/Users/luke/Downloads/impact.ttf", fontSize)
            topTextSize = font.getsize(upzero)
            bottomTextSize = font.getsize(botzero)

            while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
                fontSize = fontSize - 1
                font = ImageFont.truetype("/mnt/c/Users/luke/Downloads/impact.ttf", fontSize)
                topTextSize = font.getsize(upzero)
                bottomTextSize = font.getsize(botzero)

            # find top centered position for top text
            topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
            topTextPositionY = 0
            topTextPosition = (topTextPositionX, topTextPositionY)

            # find bottom centered position for bottom text
            bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
            bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
            bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

            draw = ImageDraw.Draw(money)

            outlineRange = int(fontSize/15)
            for x in range(-outlineRange, outlineRange+1):
                for y in range(-outlineRange, outlineRange+1):
                        draw.text((topTextPosition[0]+x, topTextPosition[1]+y), upzero, (0,0,0), font=font)
                        draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), botzero, (0,0,0), font=font)

                draw.text(topTextPosition, upzero, (255,255,255), font=font)
                draw.text(bottomTextPosition, botzero, (255,255,255), font=font)

                money.save('/mnt/c/Users/luke/reducehrsavedol/galleries/{}.png'.format(len(lisgal)))

        

            return send_file('/mnt/c/Users/luke/reducehrsavedol/galleries/{}.png'.format(len(lisgal)), mimetype='image/gif')

        return '''
                <!DOCTYPE html>
            <html>
            <head>
              <title>pay less save dollars</title>
              <script src="https://unpkg.com/vue"></script>
              <script src="node_modules/vue/vue.min.js"></script>
              <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap/dist/css/bootstrap.min.css"/>
              <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap-vue@latest/dist/bootstrap-vue.css"/>
              <meta name="viewport" content="width=device-width, initial-scale=1">
              </head>
              <body>
                <h1>spent less, save dollars</h1><br>

                <form method="POST">
                  <fieldset>
                      number of employees: <input type="text" name="ameploy"><br>
                      daily avg pay: <input type="text" name="avgpay"><br>
                      <input type="submit" value="Submit"><br>
                      </fieldset>
                  </form></body>'''






if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5556) 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
