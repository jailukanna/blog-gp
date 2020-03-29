---
title: pygtk example xkcd viewer (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'xkcd viewer'

Functions in program: 
* `def get_image():`
* `def get_image_num(num):`

Modules used in program: 
* `import gobject`
* `import os`
* `import gtk`
* `import pygtk`

## python xkcd viewer

Python pygtk example: xkcd viewer

```python
"""
See this gist: https://gist.github.com/1984424
Download both of these gists. save it in same folder. The down_xkcd.py will download all the xkcd comics from net. And view comics using xkcd_viewer.py
"""

import pygtk
import gtk
from PIL import Image
import os
from random import randint
from threading import Thread
import gobject
gtk.gdk.threads_init()

files = os.listdir("down_xkcd")
f_dict={}
for f in files:
    f_dict[int(f.split("-")[0])]=f.split("-")[-1]
nums = sorted(f_dict.keys())

class MainWindow:
    def __init__(self):
        self.window = gtk.Window()
        self.window.set_title('xkcd')
        self.window.connect("destroy", self.close_application)
        #self.window.set_size_request(800,600)
        
        self.box = gtk.VBox(False, 0)
        self.window.add(self.box)
        self.box.show()
        
        self.hbox = gtk.HBox(False,0)
        self.box.pack_start(self.hbox, False, False, 3)
        self.hbox.show()
        
        self.but1 = gtk.Button("previous")
        self.but1.show()
        self.but1.connect("clicked",self.pre_image)
        self.hbox.pack_start(self.but1)
        
        self.but2 = gtk.Button("random")
        self.but2.show()
        self.but2.connect("clicked",self.rand_image)
        self.hbox.pack_start(self.but2)
        
        self.but3 = gtk.Button("next")
        self.but3.show()
        self.but3.connect("clicked",self.next_image)
        self.hbox.pack_start(self.but3)
        
        self.imgbox = gtk.HBox(False,0)
        #self.imgbox.set_size_request(800,550)
        #self.sw1.add(self.imgbox)
        self.box.pack_start(self.imgbox, False, False, 0)
        self.imgbox.show()
        """
        self.sw1 = gtk.ScrolledWindow()
        self.sw1.set_policy(gtk.POLICY_AUTOMATIC, gtk.POLICY_AUTOMATIC)
        self.sw1.show()
        #self.imgbox.pack_start(self.sw1, False, False, 0)
        self.imgbox.add(self.sw1)
        """
        self.image = gtk.Image()
        self.imgbox.pack_start(self.image, True, True, 0)
        #self.sw1.add(self.image)
        
        self.img_file = get_image()
        print(self.img_file)
        print(os.path.exists(self.img_file))
        self.image.set_from_file(self.img_file)
        self.image.show()
    
        
        self.window.show()
    
    def next_image(self, widget=None, data=None, index=None):
        if index is None:
            num = int(self.img_file.split("/")[-1].split("-")[0])
            index = nums.index(num)
            print(index,"index")
        if index == len(nums)-1:
            return
        index = index + 1
        num = nums[index]
        file_path = get_image_num(num)
        if file_path is None:
            self.next_image(index=index)
        gobject.idle_add(self.ch_img,file_path)
        print(file_path)
    
    def ch_img(self,file_path):
        print("ch_img",file_path)
        self.img_file = file_path
        self.image.set_from_file(self.img_file)
        self.image.show()
        title = "-".join(["xkcd",file_path.split("/")[-1]])
        self.window.set_title(title)
       
    def pre_image(self, widget=None, data=None, index=None):
        if index is None:
            num = int(self.img_file.split("/")[-1].split("-")[0])
            index = nums.index(num)
            print(index,"index")
        if index == 0:
            return
        index = index - 1
        num = nums[index]
        file_path = get_image_num(num)
        if file_path is None:
            self.pre_image(index=index)
        gobject.idle_add(self.ch_img,file_path)
    
    def rand_image(self, widget=None, data=None):
        index = randint(0,nums[-1])
        file_path = get_image_num(index)
        if file_path is None:
            self.rand_image()
        gobject.idle_add(self.ch_img,file_path)
        
    def close_application(self, widget, data=None):
        gtk.main_quit()

def get_image_num(num):
    print(num)
    try:
        img_file = "-".join([str(num),f_dict[num]])
        file_path = os.path.join(os.path.abspath("down_xkcd"),img_file)
    except KeyError:
        file_path = None
    return file_path
        
def get_image():
    print(nums[-1])
    img_file = "-".join([str(nums[-1]),f_dict[nums[-1]]])
    file_path = os.path.join(os.path.abspath("down_xkcd"),img_file)
    return file_path
        
if __name__ == '__main__':
    MainWindow()
    gtk.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
