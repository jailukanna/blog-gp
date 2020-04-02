---
title: PIL example nasagif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'nasagif'

Functions in program: 
* `def reqspace():`

Modules used in program: 
* `import arrow`
* `import json`
* `import getpass`
* `import datetime`
* `import shutil`
* `import arrow`
* `import requests `
* `import imageio`
* `import PIL`
* `import os`
* `import shutil`
* `import getpass`
* `import requests`
* `import json`

## python nasagif

Python PIL example: nasagif

```python
from flask import Flask, request, jsonify
import json
import requests
import getpass
import shutil
import os
import PIL
import imageio

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

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

myusr = getpass.getuser()
homepath = '/home/{}'.format(myusr)
app = Flask(__name__)
from urllib.parse import urlparse

import arrow


       
@app.route('/reqspace')
def reqspace():
    thedate= request.args.get('thedate')
    msg = request.args.get('msg')

    marsreq = requests.get('https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos?earth_date=' + thedate + '&api_key=kPbV6U8YCKxTvBXc65wGRjJdgVIsvIsNJ0rHYjcE')
    
    marjs = marsreq.json()
        #url = "http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/01332/opgs/ed/fcam/FLB_515753675EDR_F0540938FHAZ00206M_.JPG"
    a = urlparse(marjs['photos'][0]['img_src'])
#print(a.path)
    #print(os.path.basename(a.path))
    response = requests.get(marjs['photos'][0]['img_src'], stream=True)
    with open('{}'.format(os.path.basename(a.path)), 'wb') as out_file:
        shutil.copyfileobj(response.raw, out_file)
        del response
    #return(os.path.basename(a.path))
    #return(jsonify(marjs['photos'][0]))
    rqnat = requests.get('https://epic.gsfc.nasa.gov/api/natural/date/' + thedate + '?api_key=kPbV6U8YCKxTvBXc65wGRjJdgVIsvIsNJ0rHYjcE')
    rjsn = rqnat.json()
    lenet = len(rjsn)
    #somnewd = arrow.get(rjsn[0]['date'])
    
    rqnat = requests.get('https://epic.gsfc.nasa.gov/api/natural/date/{}?api_key=kPbV6U8YCKxTvBXc65wGRjJdgVIsvIsNJ0rHYjcE'.format(thedate))
    rjsn = rqnat.json()
    lenet = len(rjsn)
    somnewd = arrow.get(rjsn[0]['date'])
    
    images = []


    for somn in range(0, lenet):
        urlearth = 'https://epic.gsfc.nasa.gov/archive/natural/' + somnewd.strftime('%Y/%m/%d') + '/png/' + rjsn[somn]['image'] + '.png'
        print(urlearth )
        response = requests.get(urlearth, stream=True)
        with open('{}.png'.format(rjsn[somn]['image']), 'wb') as out_file:
            shutil.copyfileobj(response.raw, out_file)
            del response
        earim = PIL.Image.open('{}.png'.format(rjsn[somn]['image']))
        earsm = earim.resize([512,512])

        marim = PIL.Image.open('{}'.format(os.path.basename(a.path)))


        marsm = marim.resize([512,512])

        earimg = earsm.convert("RGBA")
        marimg = marsm.convert("RGBA")

        alphaBlended1 = Image.blend(earimg, marimg, alpha=.3)
        
<<<<<<< HEAD
        font = ImageFont.truetype("Monospace.ttf", 72)
=======
        font = ImageFont.truetype("/home/wcmckee/Downloads/Monospace.ttf", 72)
>>>>>>> a13b6a49e25cf7c0c2c1d43ac7474cf2e42b96a3

        #img = Image.open("sample_in.jpg")
        draw = ImageDraw.Draw(alphaBlended1)
        # font = ImageFont.truetype(<font-file>, <font-size>)
        #font = ImageFont.truetype("sans-serif.ttf", 16)
        # draw.text((x, y),"Sample Text",(r,g,b))
<<<<<<< HEAD
        draw.text((0, 0),thedate + ' \n' + msg[0:12] + ' \n' + msg[12:24] + ' \n' + msg[24:36],(255, 255, 255),font=font)
        #img.save('sample-out.jpg')
        alphaBlended1.save('gif/{}-{}.png'.format(thedate, somn))
=======
        draw.text((0, 0),"Sample Text",(255,255,255),font=font)
        #img.save('sample-out.jpg')
        alphaBlended1.save('gif/test-{}.png'.format(somn))
>>>>>>> a13b6a49e25cf7c0c2c1d43ac7474cf2e42b96a3
        
        
        
        
    #return('ok')
    #gif the blend

    #images = []
    #mport imageio
    earthspin = os.listdir('gif')
    for filename in earthspin:
        images.append(imageio.imread('gif/' + filename))
    imageio.mimsave('{}.gif'.format(thedate), images)
    return send_file('{}.gif'.format(thedate), mimetype='image/gif')
'''
    
    #urlearth = 'https://epic.gsfc.nasa.gov/archive/natural/' + somnewd.strftime('%Y/%m/%d') + '/png/' + rjsn[0]['image'] + '.png'
    #response = requests.get(urlearth, stream=True)
    #with open('{}.png'.format(rjsn[0]['image']), 'wb') as out_file:
    #    shutil.copyfileobj(response.raw, out_file)
    #    del response
        
    
    
    
    
    images = []
    for filename in imglis:
        images.append(imageio.imread(filename))
    imageio.mimsave('earth/movie.gif', images)
    
    img = alphaBlended1
    
    histor = requests.get('http://history.muffinlabs.com/date/{}'.format(somnewd.strftime('%m/%d')))

    hisjs = histor.json()

    evens = (hisjs['data']['Events'])
    evenlis = list()

    for ev in evens:
        evenlis.append(ev['text'])
        
    upzero = ''.join(evenlis)

    #upzero = "> omgnazi\n > This is not normal \n > The President is bad. \n > We are good, but also angry"


    botzero = "#GLM"

    imageSize = img.size

            # find biggest font size that works
    fontSize = int(imageSize[1]/2)
    font = PIL.ImageFont.truetype("/home/wcmckee/Downloads/Monospace.ttf")
    topTextSize = font.getsize(upzero)
    bottomTextSize = font.getsize(botzero)

    fontSize = fontSize - 1
    font = PIL.ImageFont.truetype("/home/wcmckee/Downloads/Monospace.ttf")
    topTextSize = font.getsize(upzero)
    bottomTextSize = font.getsize(botzero)

            # find top centered position for top text
    topTextPositionX = 20
    topTextPositionY = 10
    topTextPosition = (topTextPositionX, topTextPositionY)

            # find bottom centered position for bottom text
    bottomTextPositionX = 200
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -20
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(img)

    outlineRange = int(fontSize/2)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
            draw.text((topTextPosition[0]+x, topTextPosition[1]+y), upzero, (255,255,255), font=font)
            draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), botzero, (255,255,255), font=font)

    draw.text(topTextPosition, upzero, (0,0,0), font=font)
    draw.text(bottomTextPosition, botzero, (0,0,0), font=font)

    
    #               album_path="me/photos")
    strippng = os.path.basename(a.path)
    strpng = strippng.replace('.png', '')
    aljpg = img.convert('RGB')
    aljpg.save('{}'.format(strippng))
    #aljpg.save('{}'.format(strippng))
    return send_file(strippng, mimetype='image/gif')
       
    #return('<img src="{}">'.format(strippng))

'''


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5555) 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
