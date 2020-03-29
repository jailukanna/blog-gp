---
title: pygtk example vboxbackground (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'vboxbackground'


Modules used in program: 
* `import os`
* `import gtk`
* `import pygtk`

## python vboxbackground

Python pygtk example: vboxbackground

```python
#!/usr/bin/python

import pygtk
pygtk.require('2.0')
import gtk
import os

class VBoxBackground:
    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.set_border_width(10)
        self.window.set_title("VBox Background")

        self.container = gtk.HBox()

        self.window.connect("delete_event", self.delete_event)
        self.window.connect("destroy", self.destroy)

        self.start_button = gtk.Button("Start VM")
        self.start_button.connect("clicked", self.start_vm)

        self.stop_button = gtk.Button("PowerOFF VM")
        self.stop_button.connect("clicked", self.poweroff_vm)

        self.combobox = gtk.combo_box_new_text()
        self.vms()

        self.container.add(self.start_button)
        self.container.add(self.combobox)
        self.container.add(self.stop_button)

        self.window.add(self.container)
        self.window.show_all()

    def start_vm(self, widget):
        os.popen("VBoxManage startvm " + self.vm_atual() + " --type=vrdp")
        print("Starting VM " + self.vm_atual())

    def poweroff_vm(self, widget):
        os.popen("VBoxManage controlvm " + self.vm_atual() + " poweroff")
        print("Stoping VM " + self.vm_atual())

    def delete_event(self, widget, event, data=None):
        return False

    def vms(self):
        lista = os.popen("VBoxManage list vms").readlines()
        [self.combobox.append_text(x.split('"')[1]) for i,x in enumerate(lista) if i>3]

    def vm_atual(self):
        return self.combobox.get_model()[self.combobox.get_active()][0]

    def destroy(self, widget, data=None):
        gtk.main_quit()

    def main(self):
        gtk.main()

if __name__ == "__main__":
    vboxb = VBoxBackground()
    vboxb.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
