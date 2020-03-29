---
title: pygtk example uzbl-lame (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'uzbl-lame'

Functions in program: 
* `def plugged_event(widget):`

Modules used in program: 
* `import subprocess`
* `import os`
* `import gtk,sys`
* `import pygtk`
* `import string`

## python uzbl-lame

Python pygtk example: uzbl-lame

```python
#!/usr/bin/python

# some docs: http://www.pygtk.org/pygtk2tutorial/sec-PlugsAndSockets.html

import string

import pygtk
pygtk.require('2.0')
import gtk,sys
import os
import subprocess

def plugged_event(widget):
	print("I (", widget, ") have just had a plug inserted!")

class UzblLame():
	def __init__(self):
		self.window = gtk.Window()
		self.vbox = gtk.VBox()
		self.socket = gtk.Socket()
		self.btnBackward = gtk.Button("<<<")
		self.btnForward = gtk.Button(">>>")
		self.address = gtk.Entry()
		self.btnGo = gtk.Button("*")
		self.toolbar = gtk.HBox()
		self.toolbar.pack_start(self.btnBackward, False, True)
		self.toolbar.pack_start(self.btnForward, False, True)
		self.toolbar.pack_end(self.btnGo, False, True)
		self.toolbar.pack_end(self.address, True, True)

		self.vbox.expand = False
		self.vbox.pack_end(self.socket)
		self.vbox.pack_start(self.toolbar, False, True)
		self.window.add(self.vbox)
		self.socketId = self.socket.get_id()

	def engage(self, args):
		self.window.connect("destroy", lambda w: gtk.main_quit())
		self.socket.connect("plug-added", plugged_event)
		subprocess.Popen(["uzbl", "-s", str(self.socketId)] + args)
		self.window.show_all()

args = []
for i in range(len(sys.argv)):
	if i == 0: continue
	if sys.argv[i] == "-s":
		i += 1;
		socket.add_id(long(sys.argv[i]))
		continue
	args.append(sys.argv[i])
	
lame = UzblLame()
lame.engage(args)
gtk.main()

# vim:tabstop=2

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
