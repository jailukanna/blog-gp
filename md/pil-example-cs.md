---
title: pil example cs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'cs'

Functions in program: 
* `def gfx():`
* `def status():`
* `def get_status():`
* `def get_gfx():`
* `def serve_pil_image(pil_img):`

Modules used in program: 
* `import StringIO`
* `import sys`
* `import socket`
* `import urllib`

## python cs

Python pil example: cs

```python
from flask import Flask, render_template, jsonify, send_file
from PIL import Image, ImageDraw, ImageFont
import urllib
import socket
import sys
import StringIO

app = Flask(__name__)

def serve_pil_image(pil_img):
    img_io = StringIO.StringIO()
    pil_img.save(img_io, 'PNG')
    img_io.seek(0)
    return send_file(img_io, mimetype='image/png')

def get_gfx():
    base = Image.open('banner.png').convert('RGBA')
    txt = Image.new('RGBA', base.size, (255,255,255,0))
    fnt = ImageFont.truetype('clacon.ttf', 28)
    fnt_small = ImageFont.truetype('clacon.ttf', 25)
    fnt_xsmall = ImageFont.truetype('clacon.ttf', 21)
    d = ImageDraw.Draw(txt)
    d.text((10,10), str(get_status()['name']), font=fnt_small, fill=(37, 0, 117, 255))
    d.text((10,85), "Map: " + str(get_status()['map']), font=fnt, fill=(37, 0, 117, 255))
    d.text((10,108), "Players: " + str(get_status()['players']) + "/" + str(get_status()['max_players']), font=fnt, fill=(37, 0, 117, 255))
    d.text((10,146), "Type: " + str(get_status()['server_type']), font=fnt_xsmall, fill=(37, 0, 117, 255))
    d.text((10,161), "Platform: " + str(get_status()['platform']), font=fnt_xsmall, fill=(37, 0, 117, 255))
    out = Image.alpha_composite(base, txt)
    return out

def get_status():
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    server_address = ('cs.hexor.ru', 27015)
    request = ''.join(
        chr(x) for x in [0xFF, 0xFF, 0xFF, 0xFF, 0x54, 0x53, 0x6F,
            0x75, 0x72, 0x63, 0x65, 0x20, 0x45, 0x6E, 0x67, 0x69,
            0x6E, 0x65, 0x20, 0x51, 0x75, 0x65, 0x72, 0x79, 0x00])

    try:
        sock.sendto(request, server_address)
        data, server = sock.recvfrom(4096)
        data = filter(None, data.split(b'\x00'))

        if len(data[5]) == 2:
            players = int(data[5][0].encode('hex'), 16)
            max_players = int(data[5][1].encode('hex'), 16)
        else:
            players = '0'
            max_players = int(data[5].encode('hex'), 16)

        if data[6][1] == 'l':
            platform = 'linux'
        elif data[6][1] == 'w':
            platform = 'windows'
        else:
            platform = 'mac'

        if data[6][0] == 'd':
            server_type = 'dedicated'
        else:
            server_type = 'listing'

        response = {
            "name": data[0][6:],
            "map": data[1],
            "folder": data[2],
            "game": data[3],
            "players": players,
            "max_players": max_players,
            "platform": platform,
            "server_type": server_type,
        }
        return response
    finally:
        sock.close()

@app.route("/status")
def status():
    return jsonify(get_status())

@app.route("/gfx.png")
def gfx():
    return serve_pil_image(get_gfx())

if __name__ == "__main__":
    app.run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
