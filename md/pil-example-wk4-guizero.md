---
title: pil example wk4-guizero (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'wk4-guizero'

Functions in program: 
* `def fetch_pokemon():`

## python wk4-guizero

Python pil example: wk4-guizero

```python
from guizero import App, TextBox, PushButton, Picture, Text   #simple gui
from pokebase import pokemon
from requests import get   #get method will fetch data from a web page
from PIL import Image  #PIL is for image manipulation
from io import BytesIO


def fetch_pokemon():

    # get the name from thre input box
    name = input_box.value

    try:

        # create object called poke populated with info for named pokemon
        poke = pokemon(name) 

        # create object containing just the content of the URL ..
        pic = get(poke.sprites.front_default).content

        # create an object called image containing processed image data (that we can look at)
        image = Image.open(BytesIO(pic)) # BytesIO function then Image.open function? Don't quite understand

        # save onto file system
        image.save('poke.gif') # apply method save (from PIL?) to object 'image'

        #update the image displayed
        icon.value='poke.gif'

        # heigh and weight
        text_height.value = 'Height = ' + str(poke.height)
        text_weight.value = 'Weight = ' + str(poke.weight)

        text_message.value = ' '
        
    except:
        
        # can there be something more specific to the pokemon call?
        text_message.value = 'POKEMON NOT FOUND; PLEASE TRY AGAIN!'        
    
# create the App
#myapp = App()
app = App(title='Pokemon Fetcher', width=400, height=300)

# add a text input box
input_box = TextBox(app, text='Name')

# add an image
icon = Picture(app, image="poke.gif")

# add a button
button = PushButton(app, command=fetch_pokemon, text='Submit')

# add display of height
text_height = Text(app, text="Height")

# add display of weight
text_weight = Text(app, text="Weight")  #string here is default

# message
text_message = Text(app, text=" ")

# display app on the screen NB does not seem to need this (why?)!
app.display    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
