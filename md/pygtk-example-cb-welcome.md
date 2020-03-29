---
title: pygtk example cb-welcome (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'cb-welcome'


Modules used in program: 
* `import string`
* `import gtk`
* `import sys`
* `import pynotify`
* `import pygtk`
* `import os`

## python cb-welcome

Python pygtk example: cb-welcome

```python
#!/usr/bin/env python
# cb-fortune
# ----------
# A silly little python script for testing changes to notify-osd settings.
# It kills notify-osd before sending a fortune notification. Requires python-notify & fortune.

import os
#if DISPLAY is not set, then set it to default ':0.0'
if len( os.getenv( 'DISPLAY', '' ) ) == 0:
	os.putenv( 'DISPLAY', ':0.0' )

import pygtk
pygtk.require('2.0')
import pynotify
import sys
import gtk
import string

if __name__ == '__main__':
    if not pynotify.init("cb-fortune"):
        sys.exit(1)
    if len(sys.argv)==1:
        os.popen("pkill notify-osd")
        title = "Statler says,"
        p = os.popen("/usr/games/fortune -s | sed 's/^[ \t]*//'|sed 's/[ \t]/ /'|sed 's/^A:/\\nA:/'|sed 's/^--/\\n--/'","r")
        message = p.read()
        while 0:
            line = p.readline()
	    print(line)
            if not line: break
            message = message + line
        image = "/usr/share/pixmaps/statler.png"
        n = pynotify.Notification(title, message, image)
        n.set_urgency(pynotify.URGENCY_LOW)
        n.set_timeout(10000)
        if not n.show():
            print("Failed to send notification")
            sys.exit(1)
        sys.exit()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
