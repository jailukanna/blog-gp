---
title: PIL example meme (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'meme'

Functions in program: 
* `def json_example():`
* `def form_example():`
* `def query_example():`
* `def meme():`
* `def formmeme():`
* `def tweetmeme():`
* `def colormeme():`
* `def colorme():`

Modules used in program: 
* `import random`
* `import xmltodict`
* `import subprocess`
* `import time`
* `import configparser`
* `import getpass`
* `import arrow`
* `import os`
* `import requests`
* `import glob`
* `import arrow`
* `import tweepy`
* `import json`
* `import PIL`
* `import getpass`
* `import os`
* `import shutil`
* `import requests`

## python meme

Python PIL example: meme

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
import requests
import os
import arrow
import getpass
import configparser
import time
import subprocess
import xmltodict
import random
myusr = getpass.getuser()
app = Flask(__name__) #create the Flask app
Bootstrap(app)

@app.route('/colorme')
def colorme():
    #reqran = requests.get('http://www.colourlovers.com/api/colors/random')
    #colxml = xmltodict.parse(reqran.text)
    #thecolor = colxml['colors']['color']['rgb']


    memename = request.args.get('memename')


    toptext = request.args.get('toptext')
    toptext = toptext.upper()

    bottomtext = request.args.get('bottomtext')
    bottomtext = bottomtext.upper()

    colortype = request.args.get('fontcolor')

    if colortype == 'white':
        redoth = 0
        greenoth = 0
        blueoth = 0

        bluered = 255
        bluegreen = 255
        blueblue = 255

    else:
        redoth = random.randint(50,150)
        greenoth = random.randint(50,100)
        blueoth = 60

        bluered = random.randint(100,140)
        bluegreen = 170
        blueblue = random.randint(200,255)


    #user = 'william'

    iffil = os.listdir('/home/{}/artctrl/memelol/galleries'.format(myusr))


        #img = Image.open('/home/{}/rbnz/meme/galleries/{}/{}'.format(myusr, user, ranmeme))

    #Two-Buttons
    img = Image.open(memename)

    imageSize = img.size

    # find biggest font size th90t works
    fontSize = int(imageSize[1]/5)
    font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
    topTextSize = font.getsize(toptext)
    bottomTextSize = font.getsize(bottomtext)

    while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
        fontSize = fontSize - 1
        font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
        topTextSize = font.getsize(toptext)
        bottomTextSize = font.getsize(bottomtext)

    # find top centered position for top text
    topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
    topTextPositionY = 0
    topTextPosition = (topTextPositionX, topTextPositionY)

    # find bottom centered position for bottom text
    bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(img)

    outlineRange = int(fontSize/15)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (redoth,greenoth,blueoth), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottomtext, (redoth,greenoth,blueoth), font=font)

        draw.text(topTextPosition, toptext, (bluered,bluegreen,blueblue), font=font)
        draw.text(bottomTextPosition, bottomtext, (bluered,bluegreen,blueblue), font=font)
        #home/{}/rbnz/meme/galleries/{}/{}'.format(myusr, user, ranmeme)
        img.save('/home/{}/artctrl/memelol/galleries/{}.jpg'.format(myusr, len(iffil) + 1))

        #img.save('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/{}.jpg'.format(myusr, user, memename))
    return('<img src="http://10.1.1.231/galleries/{}.jpg">'.format(len(iffil) +1))


@app.route('/colormeme')
def colormeme():
    reqran = requests.get('http://www.colourlovers.com/api/colors/random')
    colxml = xmltodict.parse(reqran.text)
    thecolor = colxml['colors']['color']['rgb']
    redoth = 255 - int(thecolor['red'])
    greenoth = 255 - int(thecolor['green'])
    blueoth = 255 - int(thecolor['blue'])

    memename = request.args.get('memename')


    toptext = request.args.get('toptext')
    toptext = toptext.upper()

    bottomtext = request.args.get('bottomtext')
    bottomtext = bottomtext.upper()
    #user = 'william'

    iffil = os.listdir('/home/pi/loungebackup/pi/memesite/will/galleries/william/todaystuff')


        #img = Image.open('/home/{}/rbnz/meme/galleries/{}/{}'.format(myusr, user, ranmeme))

    #Two-Buttons
    img = Image.open(memename)

    imageSize = img.size

    # find biggest font size th90t works
    fontSize = int(imageSize[1]/5)
    font = ImageFont.truetype("/home/pi/Downloads/impact.ttf", fontSize)
    topTextSize = font.getsize(toptext)
    bottomTextSize = font.getsize(bottomtext)

    while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
        fontSize = fontSize - 1
        font = ImageFont.truetype("/home/pi/Downloads/impact.ttf", fontSize)
        topTextSize = font.getsize(toptext)
        bottomTextSize = font.getsize(bottomtext)

    # find top centered position for top text
    topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
    topTextPositionY = 50
    topTextPosition = (topTextPositionX, topTextPositionY)

    # find bottom centered position for bottom text
    bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(img)

    outlineRange = int(fontSize/15)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (redoth,greenoth,blueoth), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottomtext, (redoth,greenoth,blueoth), font=font)

        draw.text(topTextPosition, toptext, (int(thecolor['red']),int(thecolor['green']),int(thecolor['blue'])), font=font)
        draw.text(bottomTextPosition, bottomtext, (int(thecolor['red']),int(thecolor['green']),int(thecolor['blue'])), font=font)
        #home/{}/rbnz/meme/galleries/{}/{}'.format(myusr, user, ranmeme)
        img.save('/home/pi/loungebackup/pi/memesite/will/galleries/william/todaystuff/{}.jpg'.format(len(iffil) + 1))

        #img.save('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/{}.jpg'.format(myusr, user, memename))
    return('image created')

@app.route('/tweetmeme')
def tweetmeme():


    user = request.args.get('user')
    tweetdetail = request.args.get('tweetdetail')
    #memename = request.args.get('memename')

    list_of_files = glob.glob('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/*'.format(myusr, user)) # * means all if need specific format then *.csv
    thememe = max(list_of_files, key=os.path.getctime)
    #print(latest_file)
    #thememe = ('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/{}.jpg'.format(myusr, user, memename))
    with open('/home/{}/config.txt'.format(myusr), 'r') as wckz:
        allkey = wckz.readlines()
        OAUTH_TOKEN = allkey[0]
        #print(OAUTH_TOKEN)
        OAUTH_SECRET = allkey[1]
        CONSUMER_KEY = allkey[2]
        CONSUMER_SECRET = allkey[3]


    # In[240]:

    #OAUTH_TOKEN


    # In[112]:

    auth = tweepy.OAuthHandler(CONSUMER_KEY.strip('\n'), CONSUMER_SECRET.strip('\n'))
    auth.set_access_token(OAUTH_TOKEN.strip('\n'), OAUTH_SECRET.strip('\n'))


    # In[113]:

    api = tweepy.API(auth)
    api.update_with_media(thememe, status= tweetdetail)
    return('Tweet Success')





@app.route('/formblogpost')
def formmeme():
    if request.method == 'POST':


        defpath = request.args.get('blogpath')

        tagblog = request.args.get('postag')
        imgorigin = request.args.get('imgpath')
        nameofblogpost = request.args.get('postname')


        # In[2]:

        myusr = getpass.getuser()


        # In[3]:

        raw = arrow.now()


        # In[4]:

        yraw = raw.strftime("%Y")
        mntaw = raw.strftime("%m")
        dytaw = raw.strftime("%d")


        # In[5]:

        #dytaw = str(28)


        # In[6]:

        fulda = yraw + '/' + mntaw + '/' + dytaw


        # In[7]:

        fultim = fulda + ' ' + raw.strftime('%H:%M:%S')


        # In[8]:

        arnow = arrow.now()


        # In[9]:

        curyr = arnow.strftime('%Y')


        # In[10]:

        curmon = arnow.strftime('%m')


        # In[11]:

        curday = arnow.strftime('%d')


        # getfloat() raises an exception if the value is not a float
        # getint() and getboolean() also do this for their respective types


        # In[108]:

        #artctrlpath = '/home/{}/git/artctrl-stage'.format(myusr)


        # In[109]:

        galerdir = ('{}/galleries/'.format(defpath))


        # In[110]:

        pathdir = ('{}/posts/'.format(defpath))


        # In[111]:

        oslispa = os.listdir(pathdir)


        # In[112]:

        galdir = os.listdir(galerdir)


        # In[113]:

        if curyr in galdir:
            pass
        else:
            os.mkdir(galerdir + curyr)


        # In[114]:

        mondir = os.listdir(galerdir + curyr)


        # In[115]:

        if curmon in mondir:
            pass
        else:
            os.mkdir(galerdir + curyr + '/' + curmon)


        # In[116]:

        daydir = os.listdir(galerdir + curyr + '/' + curmon )


        # In[117]:

        fridpath = ('{}{}/{}/{}').format(galerdir, curyr, curmon, curday)


        # In[118]:

        fulldaypath = (galerdir + curyr + '/' + curmon + '/' + curday)


        # In[119]:

        if curday in daydir:
            pass
        else:
            os.mkdir(galerdir + curyr + '/' + curmon + '/' + curday)


        # In[125]:

        daypost = ('{}/posts/{}.md'.format(defpath, nameofblogpost))


        # In[ ]:




        # In[126]:

        daymetapost = ('{}/posts/{}.meta'.format(defpath, nameofblogpost))


        # In[127]:

        daymdpost = ('{}/posts/{}.md'.format(defpath, nameofblogpost))


        # In[137]:

        subprocess.call('scp -r {}/*.png {}'.format(imgorigin, fridpath), shell=True)


        # In[145]:

        todayart = os.listdir(fulldaypath)


        # In[146]:

        todayart.sort()


        # In[147]:

        gallerpath = ('/galleries/{}/{}/{}/'.format(curyr, curmon, curday))


        # In[148]:

        urlpatz = 'http://artctrl.me'


        # In[149]:

        patchurl = urlpatz + gallerpath


        # In[150]:

        if nameofblogpost not in oslispa:

            with open(daymetapost, 'w') as daympo:
                daympo.write('.. title: {}\n.. slug: {}\n.. date: {}\n.. tags: {}\n.. link:\n.. description:\n.. type: text'.format(nameofblogpost, nameofblogpost, fultim, tagblog))

            with open(daymdpost, 'w') as daymark:
                for toar in todayart:

                    daymark.write('![{}]({}{})\n\n'.format(toar.replace('.png', ''), gallerpath, toar))

                    #api.update_with_media('{}{}/{}'.format(gifpat, namofgifsea, ranlocgif), status='Started typing script {} {}'.format(blognam, jointag))


        return '''
                        <p>Dog Meme Generator!</p>
                          <h1>{}
                          <br>{}</h1>
                          <br><a href="http://rbnz.tech:5123/galleries/images/">Link to gallery</a>

                          '''.format(toptext, bottext)
    return '''<!DOCTYPE html>
        <html>

        <head>

      <title>Dog Meme Generator</title>
      <script src="https://unpkg.com/vue"></script>
      <script src="node_modules/vue/vue.min.js"></script>
      <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap/dist/css/bootstrap.min.css"/>
      <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap-vue@latest/dist/bootstrap-vue.css"/>
      <meta name="viewport" content="width=device-width, initial-scale=1">
      </head>
      <body>
        <p>Dog meme generator</p>
                <form method="POST">
                      top text: <input type="text" name="toptext"><br>
                      bottom text: <input type="text" name="bottext"><br>
                      <input type="submit" value="Submit"><br>
                  </form></body>'''






    # check to see if that blog post name already excist, if so error and ask for something more unique!
    #
    # input art piece writers. Shows the art then asks for input, appending the input below the artwork. Give a name for the art that is appended above.

    # In[151]:

    os.chdir(defpath)


    # In[154]:

    subprocess.call('nikola build', shell=True)


    return('everything worked')
    #return '''{}<br> {}<br> {}
    #    '''.format(toptext, memename, bottomtext)

@app.route('/meme')
def meme():
    memename = request.args.get('memename')

    toptext = request.args.get('toptext')
    toptext = toptext.upper()

    bottomtext = request.args.get('bottomtext')
    bottomtext = bottomtext.upper()
    user= request.args.get('user')

    timnow = arrow.now()
    timstr = timnow.timestamp

    galdirdir = '/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/'.format(myusr, user)



    #with open('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/default')

    img = Image.open('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/default/{}.jpg'.format(myusr, memename))

    imageSize = img.size

    # find biggest font size th90t works
    fontSize = int(imageSize[1]/5)
    font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
    topTextSize = font.getsize(toptext)
    bottomTextSize = font.getsize(bottomtext)

    while topTextSize[0] > imageSize[0]-20 or bottomTextSize[0] > imageSize[0]-20:
        fontSize = fontSize - 1
        font = ImageFont.truetype("/home/{}/Downloads/impact.ttf".format(myusr), fontSize)
        topTextSize = font.getsize(toptext)
        bottomTextSize = font.getsize(bottomtext)

    # find top centered position for top text
    topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
    topTextPositionY = 0
    topTextPosition = (topTextPositionX, topTextPositionY)

    # find bottom centered position for bottom text
    bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(img)

    outlineRange = int(fontSize/15)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (0,0,0), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottomtext, (0,0,0), font=font)

        draw.text(topTextPosition, toptext, (255,255,255), font=font)
        draw.text(bottomTextPosition, bottomtext, (255,255,255), font=font)
        img.save('/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/{}-{}.jpg'.format(myusr, user, memename, timstr))

        #img.save("/home/{}/memetest/galleries/{}/{}.jpg".format(myusr, usrfolz, gtm['id']))


    memedict = dict({'meme' : memename, 'toptext' : toptext.upper(), 'bottomtext' : bottomtext.upper(), 'imagepath' : '/home/{}/rbnz-tech-backup/artctrl/meme/galleries/{}/{}-{}.jpg'.format(myusr, user, memename, timstr)})
    return(jsonify(memedict))
    #return '''{}<br> {}<br> {}
    #    '''.format(toptext, memename, bottomtext)

@app.route('/query-example')
def query_example():
    toptext = request.args.get('toptext') #if key doesn't exist, returns None
    bottext = request.args['bottext'] #if key doesn't exist, returns a 400, bad request error
    reqdog = requests.get('https://dog.ceo/api/breeds/image/random')
    dogjs = (reqdog.json())
    response = requests.get(dogjs['message'], stream=True)
    o = urlparse(dogjs['message'])
    sompath = (o.path)
    thesplit = (sompath.split('/'))
    pngspli = (thesplit[-1])

    mystuff = "/home/{}/artctrl/dogmeme/galleries".format(myusr)

    #oslis = os.listdir(mystuff)
    with open('{}/{}'.format(mystuff, pngspli), 'wb') as out_file:
        shutil.copyfileobj(response.raw, out_file)
        del response
    img = Image.open('{}/{}'.format(mystuff, pngspli))

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
    topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
    topTextPositionY = 0
    topTextPosition = (topTextPositionX, topTextPositionY)

            # find bottom centered position for bottom text
    bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
    bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
    bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

    draw = ImageDraw.Draw(img)

    outlineRange = int(fontSize/15)
    for x in range(-outlineRange, outlineRange+1):
        for y in range(-outlineRange, outlineRange+1):
            draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (0,0,0), font=font)
            draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottext, (0,0,0), font=font)

        draw.text(topTextPosition, toptext, (255,255,255), font=font)
        draw.text(bottomTextPosition, bottext, (255,255,255), font=font)

        img.save('{}/images/{}'.format(mystuff, pngspli.strip('\n')))
        print(pngspli)


    return '''<h1>{}
              <br>{}</h1>
              <br><img src="images/{}">
              '''.format(toptext, bottext, pngspli)

@app.route('/form-example', methods=['GET', 'POST']) #allow both GET and POST requests
def form_example():
    if request.method == 'POST':  #this block is only entered when the form is submitted


        toptext = request.form.get('toptext') #if key doesn't exist, returns None
        bottext = request.form['bottext'] #if key doesn't exist, returns a 400, bad request error
        reqdog = requests.get('https://dog.ceo/api/breeds/image/random')
        dogjs = (reqdog.json())
        response = requests.get(dogjs['message'], stream=True)
        o = urlparse(dogjs['message'])
        sompath = (o.path)
        thesplit = (sompath.split('/'))
        pngspli = (thesplit[-1])

        mystuff = "/home/{}/artctrl/dogmeme/galleries".format(myusr)

        oslis = os.listdir(mystuff)
        with open('{}/{}'.format(mystuff, pngspli), 'wb') as out_file:
            shutil.copyfileobj(response.raw, out_file)
            del response
        img = Image.open('{}/{}'.format(mystuff, pngspli))

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
        topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
        topTextPositionY = 0
        topTextPosition = (topTextPositionX, topTextPositionY)

                # find bottom centered position for bottom text
        bottomTextPositionX = (imageSize[0]/2) - (bottomTextSize[0]/2)
        bottomTextPositionY = imageSize[1] - bottomTextSize[1] -10
        bottomTextPosition = (bottomTextPositionX, bottomTextPositionY)

        draw = ImageDraw.Draw(img)

        outlineRange = int(fontSize/15)
        for x in range(-outlineRange, outlineRange+1):
            for y in range(-outlineRange, outlineRange+1):
                draw.text((topTextPosition[0]+x, topTextPosition[1]+y), toptext, (0,0,0), font=font)
                draw.text((bottomTextPosition[0]+x, bottomTextPosition[1]+y), bottext, (0,0,0), font=font)

            draw.text(topTextPosition, toptext, (255,255,255), font=font)
            draw.text(bottomTextPosition, bottext, (255,255,255), font=font)

            img.save('{}/images/{}'.format(mystuff, pngspli.strip('\n')))
            print(pngspli)


        return '''
                <p>Dog Meme Generator!</p>
                  <h1>{}
                  <br>{}</h1>
                  <br><a href="http://rbnz.tech:5123/galleries/images/">Link to gallery</a>

                  '''.format(toptext, bottext)

    return '''<!DOCTYPE html>
<html>

<head>

  <title>Dog Meme Generator</title>
  <script src="https://unpkg.com/vue"></script>
  <script src="node_modules/vue/vue.min.js"></script>
  <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap/dist/css/bootstrap.min.css"/>
  <link type="text/css" rel="stylesheet" href="//unpkg.com/bootstrap-vue@latest/dist/bootstrap-vue.css"/>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  </head>
  <body>
    <p>Dog meme generator</p>
            <form method="POST">
                  top text: <input type="text" name="toptext"><br>
                  bottom text: <input type="text" name="bottext"><br>
                  <input type="submit" value="Submit"><br>
              </form></body>'''

@app.route('/json-example')
def json_example():
    return 'Todo...'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000) #run app in debug mode on port 5000


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
