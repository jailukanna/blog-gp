---
title: PIL example Nokia5110 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Nokia5110'


Modules used in program: 
* `import datetime # pour l'affichage de la date`
* `import subprocess # pour aller chercher l'adresse IP`
* `import Adafruit_GPIO.SPI as SPI`
* `import Adafruit_Nokia_LCD as LCD`
* `import time # pour le chronométrage des délais`

## python Nokia5110

Python PIL example: Nokia5110

```python
#!/usr/bin/python
# -*- coding:utf-8 -*-

'''

On s'amuse un peu avec un écran Nokia 5150 branché
à un Raspberry Pi.
(Affichage de texte et de formes, on affiche
aussi l'adresse IP, ainsi que la date et l'heure).

https://electroniqueamateur.blogspot.com/2019/01/afficheur-lcd-nokia-5110-et-raspberry-pi.html

'''

import time # pour le chronométrage des délais

import Adafruit_Nokia_LCD as LCD
import Adafruit_GPIO.SPI as SPI

import subprocess # pour aller chercher l'adresse IP
import datetime # pour l'affichage de la date

# PIL : Python Imaging Library
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

# Définition des broches utilisées
DC = 23
RST = 24
SPI_PORT = 0
SPI_DEVICE = 0

# déclaration d'un objet PCD8544
disp = LCD.PCD8544(DC, RST, spi=SPI.SpiDev(SPI_PORT, SPI_DEVICE, max_speed_hz=4000000))

# Intialisation; la valeur idéale pour le contraste n'est pas le même pour tous les écrans
disp.begin(contrast=60)

# on efface
disp.clear()
disp.display()

# Chargement de la police de caractères
font = ImageFont.load_default()

# -------------- animation d'ouverture ---------------------------

print("Animation d'ouverture"  # dans le terminal, pour aider le débogage)

image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))
draw = ImageDraw.Draw(image)

# rectangle blanc comme fond d'écran
draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=255, fill=255)

# pointe de tarte dont l'angle d'ouverture aumente progressivement:
for x in range(0, 91):
    draw.pieslice((22,4,62,44),-2*x,2*x,fill = 0 )
    disp.image(image)
    disp.display()
    time.sleep(0.01)

time.sleep(1.0)

# -------------- affichage de l'adresse IP -----------------------

print("Affichage de l'adresse IP" # dans le terminal, pour aider le débogage)

image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))
draw = ImageDraw.Draw(image)

# rectangle blanc comme fond d'écran
draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=255, fill=255)
# petit rectangle noir
draw.rectangle((8,8,70,22), outline=0, fill=0)
# "Adresse IP" écrite en blanc, par dessus le rectangle noir
draw.text((10,10), "Adresse IP:", font=font, fill = 255)
# Obtention de l'adresse IP
IP = subprocess.check_output(["hostname", "-I"]).split()[0]
# l'adresse IP est écrite en noir, plus bas sur l'écran
draw.text((3,28), str(IP), font=font)

# on transfert à l'écran tout ce qu'on vient de dessiner
disp.image(image)
disp.display()

time.sleep(5.0)

# ----------- affichage de la date et de l'heure ----------------

print("Affichage de la date et de l'heure" # dans le terminal, pour aider le débogage)

image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))
draw = ImageDraw.Draw(image)

# rectangle blanc comme fond d'écran
draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=255, fill=255)

# obtention de la date
x = datetime.datetime.now()

# on écrit la date
draw.text((20,6), x.strftime("%x"), font=font)
# on écrit l'heure
draw.text((20,30), x.strftime("%X"), font=font)
# droite horizontale entre la date et l'heure:
draw.line((20,24,64,24), fill = 0 )

# on transfert à l'écran tout ce qu'on vient de dessiner
disp.image(image)
disp.display()

time.sleep(5.0)

# ----------- affichage d'une déplorable pub  ----------------

print("Affichage: Electronique en amateur"  # dans le terminal, pour aider le débogage)

image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))
draw = ImageDraw.Draw(image)

# On remplit l'écran avec un fond noir
draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=0, fill=0)

# On écrit 3 lignes de texte, en blanc
draw.text((8,8), "Electronique", font=font, fill = 255)
draw.text((35,18), "en", font=font, fill = 255)
draw.text((20,28), "amateur", font=font, fill = 255)

# on transfert à l'écran tout ce qu'on vient de dessiner
disp.image(image)
disp.display()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
