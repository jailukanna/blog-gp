---
title: pygtk example hellodolly (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'hellodolly'

Functions in program: 
* `def applet_factory(applet, iid):`
* `def update_lyric():`

Modules used in program: 
* `import threading`
* `import time`
* `import random`
* `import gnomeapplet`
* `import pygtk`
* `import gtk`
* `import sys`

## python hellodolly

Python pygtk example: hellodolly

```python
#!/usr/bin/python

import sys
import gtk
import pygtk
import gnomeapplet
import random
import time
import threading

pygtk.require('2.0')

lyrics = ["I said hello, Dolly", "Well, hello, Dolly", "It's so nice to have you back where you belong", "You're lookin' swell, Dolly", "I can tell, Dolly", "You're still glowin'", "You're still crowin'", "You're still goin' strong", "I feel that room swayin'", "While the band's playin'", "One of your old favourite songs from way back when", "So take her wrap, fellas", "Find her an empty lap, fellas", "Dolly'll never go away again", "While that ole band keeps on playin'", "So golly, gee, fellas", "Find her an empty knee, fellas", "Dolly'll never go away", "I said she'll never go away", "Dolly'll never go away again"]

label = gtk.Label("init")

def update_lyric():
    label.set_label(random.choice(lyrics))
    gtk.timeout_add(60000, update_lyric)

def applet_factory(applet, iid):
    applet.add(label)
    update_lyric()
    applet.show_all()
    return True

if __name__ == '__main__':   # testing for execution
    gnomeapplet.bonobo_factory('OAFIID:hellodolly_factory',
                               gnomeapplet.Applet.__gtype__,
                               'Hello Dolly', '0.1',
                               applet_factory)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
