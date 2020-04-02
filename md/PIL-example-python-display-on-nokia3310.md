---
title: PIL example python-display-on-nokia3310 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'python-display-on-nokia3310'

Functions in program: 
* `def on_message(client, userdata, msg):`
* `def on_connect(client, userdata, flags, rc):`

Modules used in program: 
* `import json`
* `import paho.mqtt.client as mqtt`
* `import Adafruit_GPIO.SPI as SPI`
* `import Adafruit_Nokia_LCD as LCD`

## python python-display-on-nokia3310

Python PIL example: python-display-on-nokia3310

```python
#!/usr/bin/python3
# coding: utf-8

from time import gmtime, strftime

import Adafruit_Nokia_LCD as LCD
import Adafruit_GPIO.SPI as SPI

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
import paho.mqtt.client as mqtt

import json


# Raspberry Pi hardware SPI config:
DC = 23
RST = 24
SPI_PORT = 0
SPI_DEVICE = 0

# Hardware SPI usage:
disp = LCD.PCD8544(DC, RST, spi=SPI.SpiDev(SPI_PORT, SPI_DEVICE, max_speed_hz=4000000))
# Initialize library.
disp.begin(contrast=60)

font = ImageFont.load_default()

## The callback for when the client receives a CONNACK response from the server.
def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))


# The callback for when a PUBLISH message is received from the server.
def on_message(client, userdata, msg):
    datetime = strftime("%Y-%m-%d %H:%M:%S", gmtime()).split(" ")

    message = json.loads(msg.payload.decode('utf-8'))

    temperature = 'temp: {}'.format(message['temperature'])
    pressure = 'pres: {}'.format(message['pressure'])
    humidity = 'humi: {}'.format(message['humidity'])

    # Clear display.
    disp.clear()
    disp.display()
    image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))

    # Get drawing object to draw on image.
    draw = ImageDraw.Draw(image)
    # Draw a white filled box to clear the image.
    draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=255, fill=255)
    draw.text((2,0), datetime[0], font=font)
    draw.text((2,10), datetime[1], font=font)
    draw.text((2,20), temperature, font=font)
    draw.text((2,30), pressure, font=font)
    draw.text((2,40), humidity, font=font)

    # Display image.
    disp.image(image)
    disp.display()


client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

client.connect("localhost", 1883, 60)
client.subscribe("environment", qos=0)

# Blocking call that processes network traffic, dispatches callbacks and
# handles reconnecting.
# Other loop*() functions are available that give a threaded interface and a
# manual interface.
client.loop_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
