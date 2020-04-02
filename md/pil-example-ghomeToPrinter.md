---
title: pil example ghomeToPrinter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ghomeToPrinter'

Functions in program: 
* `def on_message(client, userdata, msg):`
* `def on_connect(client, userdata, flags, respons_code):`

Modules used in program: 
* `import json`
* `import paho.mqtt.client as mqtt`
* `import subprocess`
* `import io`
* `import sys`

## python ghomeToPrinter

Python pil example: ghomeToPrinter

```python
#! /usr/bin/env python
# coding: utf-8
# coding=utf-8
# -*- coding: utf-8 -*-
# vim: fileencoding=utf-8
import sys
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import io
import subprocess
import paho.mqtt.client as mqtt
import json

TOKEN = "YOURTOKEN"
HOSTNAME = "mqtt.beebotte.com"
PORT = 8883
TOPIC = "YOURCHANNEL/voice"
CACERT = "mqtt.beebotte.com.pem"


fontFile = 'NotoSansCJKjp-Medium.otf'
font = ImageFont.truetype(fontFile, 40, encoding='utf-8')

def on_connect(client, userdata, flags, respons_code):
    print('status {0}'.format(respons_code))
    client.subscribe(TOPIC)

def on_message(client, userdata, msg):
    print(msg.topic + " " + str(msg.payload))
    data = json.loads(msg.payload.decode("utf-8"))["data"]
    print(data)
    
    text = data
    w, h = font.getsize(text)
    img = Image.new('RGB', (400, 50), (255,255,255))
    draw = ImageDraw.Draw(img)
    draw.text((0, 0), text, fill=(0,0,0), font=font)
    buf = io.BytesIO()
    img.save(buf, 'PNG')

    # print(out)
    p = subprocess.Popen('lpr', stdin=subprocess.PIPE)
    p.communicate(buf.getvalue())

client = mqtt.Client()
client.username_pw_set("token:%s"%TOKEN)
client.on_connect = on_connect
client.on_message = on_message
client.tls_set(CACERT)
client.connect(HOSTNAME, port=PORT, keepalive=60)
client.loop_forever()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
