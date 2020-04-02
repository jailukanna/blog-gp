---
title: PIL example test asyncimage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test asyncimage'


Modules used in program: 
* `import requests`

## python test asyncimage

Python PIL example: test asyncimage

```python
from io import BytesIO
import requests

from kivy.config import Config
Config.set('network', 'useragent', 'Mozilla/5.0 (platform; rv:geckoversion) Gecko/geckotrail Firefox/68.0.2')

from kivy.app import App
from kivy.lang import Builder
from kivy.factory import Factory
from kivy.properties import ObjectProperty
from kivy.loader import Loader
from kivy.core.image.img_pil import ImageLoaderPIL


KV = '''
AsyncImage:
    source: 'https://estudiantes.udp.cl/wp-content/uploads/2019/01/toma-de-ramos-351x185.jpg'
'''

class Application(App):
    texture = ObjectProperty()

    def build(self):
        return Builder.load_string(KV)


if __name__ == "__main__":
    Application().run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
