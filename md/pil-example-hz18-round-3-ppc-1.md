---
title: pil example hz18-round-3-ppc-1 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'hz18-round-3-ppc-1'

Functions in program: 
* `def from_morse(s):`

Modules used in program: 
* `import base64`
* `import PIL.ImageOps`
* `import requests`
* `import re`

## python hz18-round-3-ppc-1

Python pil example: hz18-round-3-ppc-1

```python
#!/usr/bin/env python

import re
import requests
from pyzbar.pyzbar import decode
from PIL import Image
from io import BytesIO
import PIL.ImageOps
import base64

CODE = {'A': '.-',     'B': '-...',   'C': '-.-.',
        'D': '-..',    'E': '.',      'F': '..-.',
        'G': '--.',    'H': '....',   'I': '..',
        'J': '.---',   'K': '-.-',    'L': '.-..',
        'M': '--',     'N': '-.',     'O': '---',
        'P': '.--.',   'Q': '--.-',   'R': '.-.',
        'S': '...',    'T': '-',      'U': '..-',
        'V': '...-',   'W': '.--',    'X': '-..-',
        'Y': '-.--',   'Z': '--..',

        '0': '-----',  '1': '.----',  '2': '..---',
        '3': '...--',  '4': '....-',  '5': '.....',
        '6': '-....',  '7': '--...',  '8': '---..',
        '9': '----.'
        }

CODE_REVERSED = {value:key for key,value in CODE.items()}

def from_morse(s):
    return ''.join(CODE_REVERSED.get(i) for i in s.split())

cookie = None

# http://192.168.8.175:8090/
response = requests.get("http://192.168.8.175:8090")

while True:
    if cookie == None:
        cookie = response.cookies['PHPSESSID']

    base64data = response.text
    base64data = base64data[base64data.find('base64')+7:base64data.find('"></div>')]

    img = Image.open(BytesIO(base64.b64decode(base64data)))
    decoded = decode(PIL.ImageOps.invert(img))

    solution = from_morse(decoded[0].data.decode())

    response = requests.post("http://192.168.8.175:8090", data={'reto':1, 'solution': solution}, cookies={'PHPSESSID': cookie})

    flag = re.search(r'HZ\{.*\}', response.text)

    if flag:
        print("FLAG IS: " + flag.group(0))
        break


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
