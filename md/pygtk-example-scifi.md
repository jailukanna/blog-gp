---
title: pygtk example scifi (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'scifi'

Functions in program: 
* `def main():`

Modules used in program: 
* `import subprocess`
* `import commands`
* `import os`
* `import gtk`
* `import pygtk`

## python scifi

Python pygtk example: scifi

```python
#!/usr/bin/env python

import pygtk
pygtk.require('2.0')
import gtk
import os
import commands
import subprocess


class teste:
    def enter_callback(self, widget, entry):
	
        entry_text = entry.get_text()
        print("Entry contents: %s\n" % entry_text)

    def rebootar(self, button, ip_entry):
	ip = ip_entry.get_text()
	subprocess.call(["ssh", "-i", "controller_key","root@"+ip,"/etc/scripts/reiniciar.sh"])

    def renomear(self, button, ip_entry, ssid_entry):
	ip = ip_entry.get_text()
	ssid = ssid_entry.get_text()
        subprocess.call(["ssh", "-i", "controller_key","root@"+ip,"/etc/scripts/trocar_ssid.sh", ssid])

    def __init__(self):
        window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        window.set_size_request(300, 300)
        window.set_title("Of Course")
        window.connect("delete_event", lambda w,e: gtk.main_quit())

        vbox = gtk.VBox(False, 0)
        window.add(vbox)
        vbox.show()

	ssid_label = gtk.Label("SSID")
	vbox.pack_start(ssid_label, True, True, 0)
	ssid_label.show()	

        ssid_entry = gtk.Entry()
        ssid_entry.set_max_length(50)
        ssid_entry.connect("activate", self.enter_callback, ssid_entry)
        ssid_entry.select_region(0, len(ssid_entry.get_text()))
        vbox.pack_start(ssid_entry, True, True, 0)
        ssid_entry.show()

	ip_label = gtk.Label("IP")
	vbox.pack_start(ip_label, True, True, 0)
	ip_label.show()	

        ip_entry = gtk.Entry()
        ip_entry.set_max_length(50)
        ip_entry.connect("activate", self.enter_callback, ip_entry)
        ip_entry.select_region(0, len(ip_entry.get_text()))
        vbox.pack_start(ip_entry, True, True, 0)
        ip_entry.show()

        hbox = gtk.HBox(False, 0)
        vbox.add(hbox)
        hbox.show()
                                                         
        button1 = gtk.Button("Rebootar")
        button1.connect("clicked", self.rebootar, ip_entry)
        vbox.pack_start(button1, True, True, 0)
        button1.set_flags(gtk.CAN_DEFAULT)
        button1.grab_default()
        button1.show()
        window.show()

	button2 = gtk.Button("Renomear")
        button2.connect("clicked", self.renomear, ip_entry, ssid_entry)
        vbox.pack_start(button2, True, True, 0)
        button2.set_flags(gtk.CAN_DEFAULT)
        button2.grab_default()
        button2.show()
        window.show()

def main():
    gtk.main()
    return 0
if __name__ == "__main__":
    teste()
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
