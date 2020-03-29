---
title: pygtk example campbx (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'campbx'

Functions in program: 
* `def campbx_factory(applet,iid):`

Modules used in program: 
* `import json, urllib, decimal`
* `import sys`
* `import gobject`
* `import gnomeapplet`
* `import gtk`
* `import pygtk`

## python campbx

Python pygtk example: campbx

```python
#!/usr/bin/env python
import pygtk
pygtk.require('2.0')

import gtk
import gnomeapplet
import gobject

import sys
import json, urllib, decimal
class CampBXApplet(gnomeapplet.Applet):

    def fetchCurrentValue(self):
        try :
            urlobj = urllib.urlopen("http://campbx.com/api/xticker.php")
            urldata = urlobj.read()
            urlobj.close()
            value = str(json.loads(urldata)[u"Last Trade"])
            decval = decimal.Decimal(value)
            if self.lastvalue > decval :
                newvalue = value + " -"
            elif self.lastvalue < decval :
                newvalue = value + " +"
            else :
                newvalue = value + " ="
            self.lastvalue = decval
            return "$" + newvalue
        except :
            return "No Data"
    #Displays the next word to GUI. Uses set_markup to use HTML
    def displayValue(self):
        value = self.fetchCurrentValue()
        self.label.set_markup('<span foreground="white">' + value + '</span>')
        return True

    def __init__(self,applet,iid):
        self.timeout_interval = 1000 * 15 #Timeout set to 10secs
        self.applet = applet
        self.lastvalue = 0


        #self.wordsToShow = []
        #self.allWords = self.readFile(self.fileName)

        wordToShow = self.fetchCurrentValue()

        self.label = gtk.Label("")
        self.label.set_markup('<span foreground="white">' + wordToShow + '</span>')
        self.applet.add(self.label)

        self.applet.show_all()
        gobject.timeout_add(self.timeout_interval, self.displayValue)

#Register the applet datatype
gobject.type_register(CampBXApplet)

def campbx_factory(applet,iid):
    CampBXApplet(applet,iid)
    return gtk.TRUE

#Very useful if I want to debug. To run in debug mode python hindiScroller.py -d
if len(sys.argv) == 2:
    if sys.argv[1] == "-d": #Debug mode
        main_window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        main_window.set_title("Python Applet")
        main_window.connect("destroy", gtk.main_quit)
        app = gnomeapplet.Applet()
        campbx_factory(app,None)
        app.reparent(main_window)
        main_window.show_all()
        gtk.main()
        sys.exit()

#If called via gnome panel, run it in the proper way
if __name__ == '__main__':
    gnomeapplet.bonobo_factory("OAFIID:GNOME_CampBX_Factory", CampBXApplet.__gtype__, "hello", "0", campbx_factory)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
