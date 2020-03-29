---
title: pygtk example cree (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'cree'

Functions in program: 
* `def make_zalgo(text, c=100, p=10):`

Modules used in program: 
* `import random`
* `import gtk`
* `import pygtk`

## python cree

Python pygtk example: cree

```python
import pygtk
pygtk.require('2.0')

import gtk
import random

full = '''To invoke the hive-mind representing chaos.
Invoking the feeling of chaos.
With out order.
The Nezperdian hive-mind of chaos. Zalgo.for
He who Waits Behind The Wall.
ZALGO!'''

def make_zalgo(text, c=100, p=10):
    res = []

    for char in text:
        x = [char]
        for i in range(c):
            if random.randint(0, p) == 0:
                x.append(unichr(random.randint(0x300, 0x36F)))

        res.append((''.join(x)).decode('utf-8'))

    return ''.join(res)

d = gtk.MessageDialog(message_format=make_zalgo(full), type=gtk.MESSAGE_WARNING, buttons=gtk.BUTTONS_OK)
d.set_title(make_zalgo('Zalgo. He comes', p=100))
d.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
