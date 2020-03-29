---
title: pygtk example plop (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'plop'


Modules used in program: 
* `import pynotify`
* `import gtk`
* `import pygtk`
* `import sys, os, re`

## python plop

Python pygtk example: plop

```python
#!/usr/bin/env python

import sys, os, re
import pygtk

pygtk.require('2.0')
import gtk

import pynotify
from pynotify import Notification

#### Configuration area ####

# Frequency of portage status query (ms)
REFRESH_TIMEOUT = 3000

# Notification display timeout (ms)
NOTIFY_TIMEOUT  = 2000

### DO NOT MODIFY ME BELOW THIS LINE ###

class CurrentMerge:
    def __init__(self):
        self.actPkg = ""
        pynotify.init('CurrentMerge')

        self.statusIcon=gtk.StatusIcon()

        self.setIconStatus()
        self.statusIcon.set_visible(True)

        self.menu = gtk.Menu()

        mi = gtk.ImageMenuItem(gtk.STOCK_QUIT)
        mi.connect('activate', gtk.main_quit, self.statusIcon)
        self.menu.append(mi)

        self.statusIcon.connect('popup-menu', self.popupMenu, self.menu)
        self.statusIcon.connect('activate', self.activate)
        self.statusIcon.set_visible(1)


    def popupMenu(self,widget,button,time,data = None):
        if button == 3:
            if data:
                data.show_all()
                data.popup(None, None, gtk.status_icon_position_menu,
                3, time, self.statusIcon)
    

            
    def setIconStatus(self, *args):
        self.__debug("Time tick")
        res = self.actualPackage()

        if res:
            pkg, time = res
        else:
            pkg = time = False

        # TODO make it i18n-aware
        if not pkg:
            self.statusIcon.set_tooltip("No package being merged")
            self.statusIcon.set_from_stock(gtk.STOCK_MEDIA_STOP)
        else:
            self.statusIcon.set_tooltip("Currently merging: %s " % pkg)
            self.statusIcon.set_from_stock(gtk.STOCK_MEDIA_PLAY)
        
        if pkg and self.actPkg != pkg:
            self.notifyPkg(pkg, time)

        self.actPkg = pkg
        self.actTime = time

        gtk.timeout_add(REFRESH_TIMEOUT, self.setIconStatus)

    def activate(self, widget, *data):
        self.notifyPkg(self.actPkg, self.actTime)

    def notifyPkg(self, pkg, time):
        self.notify("Installing %s" % pkg, time)

    def notify(self, msg, body = ''):
        notice = Notification(msg, body, 'package')
        notice.set_timeout(2000)
        notice.show()

    def actualPackage(self):
        rawlines = os.popen('qlop -cC').readlines()

        lines = [line.strip() for line in rawlines]

        if len(lines) == 0:
            return False
        else:
            return (lines[0].replace('* ', ''), lines[-1])
    def __debug(self, msg):
        if len(sys.argv) > 1 and sys.argv[1] == 'debug':
            print("DEBUG: %s" % msg)

# END CLASS

if __name__ == '__main__':
    app = CurrentMerge()
    gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
