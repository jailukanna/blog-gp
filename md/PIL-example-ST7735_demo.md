---
title: PIL example ST7735 demo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ST7735 demo'


Modules used in program: 
* `import datetime # pour l'affichage de la date`
* `import subprocess # pour aller chercher l'adresse IP`
* `import time # pour le chronométrage des délais`
* `import Adafruit_GPIO.SPI as SPI`
* `import Adafruit_GPIO as GPIO`
* `import ST7735 as TFT`

## python ST7735 demo

Python PIL example: ST7735 demo

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

'''

Contrôle d'un écran couleur ST7735
branché à un Raspberry Pi.

Plus d'infos:
https://electroniqueamateur.blogspot.com/2019/11/ecran-couleur-spi-st7735-et-raspberry-pi.html

'''

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

import ST7735 as TFT
import Adafruit_GPIO as GPIO
import Adafruit_GPIO.SPI as SPI

import time # pour le chronométrage des délais
import subprocess # pour aller chercher l'adresse IP
import datetime # pour l'affichage de la date

WIDTH = 128
HEIGHT = 160
SPEED_HZ = 4000000

DC = 24
RST = 25
SPI_PORT = 0
SPI_DEVICE = 0

disp = TFT.ST7735(
    DC,
    rst=RST,
    spi=SPI.SpiDev(
        SPI_PORT,
        SPI_DEVICE,
        max_speed_hz=SPEED_HZ))

# définition de quelques couleurs
COUL_BLANC = (255,255,255)
COUL_NOIR = (0,0,0)
COUL_BLEU = (0,0,255)
COUL_VERT = (0,255,0)
COUL_ROUGE = (255,0,0)
COUL_CYAN = (0,255,255)
COUL_JAUNE = (255,255,0)
COUL_MAGENTA = (255,0,255);

# Initialisation de l'écran
disp.begin()

draw = disp.draw()

disp.clear(COUL_BLANC) # fond d'écran blanc

# amimation d'ouverture: pointe de tarte dont l'angle d'ouverture aumente progressivement:
for x in range(0, 91):
    draw.pieslice((14,30,114,130),-2*x,2*x,fill = COUL_ROUGE )
    disp.display()
    time.sleep(0.01)

time.sleep(1.0)

# Deuxième tableau: quelques formes géométriques

disp.clear(COUL_BLANC) # fond d'écran blanc

# cercle bleu avec contour noir
draw.ellipse((20, 10, 60, 50), outline=(COUL_NOIR), fill=(COUL_BLEU))

# carré magenta avec contour noir
draw.rectangle((70, 10, 110, 50), outline=(COUL_NOIR), fill=(COUL_MAGENTA))

# deux traits pour produire un X
draw.line((20, 60, 60, 100), fill=(COUL_ROUGE))
draw.line((21, 60, 61, 100), fill=(COUL_ROUGE))

draw.line((20, 100, 60, 60), fill=(COUL_ROUGE))
draw.line((21, 100, 61, 60), fill=(COUL_ROUGE))

#Triangle vert avec un contour noir
draw.polygon([(70, 100), (90, 60), (110, 100)], outline=(0,0,0), fill=(0,255,255))

#police de caractères par défaut
font = ImageFont.load_default()
draw.text((10,120), "Formes geometriques", font=font, fill=COUL_NOIR)

# On transfert sur l'écran l'image en mémoire
disp.display()

time.sleep(5.0)


# tableau final: adresse IP, date et heure

disp.clear(COUL_BLANC) # fond d'écran blanc
# Adresse IP écrit en bleu
draw.text((10,10), "Adresse IP:", font=font, fill = COUL_BLEU)
# Obtention de l'adresse IP
IP = subprocess.check_output(["hostname", "-I"]).split()[0]
# l'adresse IP est écrite en noir, plus bas sur l'écran
draw.text((10,30), str(IP), font=font, fill = COUL_NOIR)

# obtention de la date
x = datetime.datetime.now()
#date écrit en rouge:
draw.text((10,50), "Date:", font=font, fill = COUL_ROUGE)
# on écrit la date
draw.text((10,70), x.strftime("%x"), font=font, fill = COUL_NOIR)

#heure écrit en vert:
draw.text((10,90), "Heure:", font=font, fill = COUL_VERT)
# on écrit l'heure
draw.text((10,110), x.strftime("%X"), font=font, fill = COUL_NOIR)

# on transfert à l'écran tout ce qu'on vient de dessiner
disp.display()

time.sleep(5.0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
