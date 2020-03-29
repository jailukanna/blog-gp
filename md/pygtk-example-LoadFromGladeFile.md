---
title: pygtk example LoadFromGladeFile (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'LoadFromGladeFile'


Modules used in program: 
* `import gtk.glade`
* `import gtk`
* `import pygtk`
* `import sys`

## python LoadFromGladeFile

Python pygtk example: LoadFromGladeFile

```python
"""
LoadFromGladeFile.py
Description: load the glade file easily
Author: Prasanna Gautam
Info: Works with libglade for the .glade file
"""
import sys
import pygtk
pygtk.require('2.0')
import gtk
import gtk.glade

class ImageViewer:
    def __init__(self,gladeFile):
        self.gladefile = gladeFile
        self.wTree = gtk.glade.XML(self.gladefile, "MainWindow")
        self.window = self.wTree.get_widget("MainWindow")
        self.window.connect('destroy',gtk.main_quit)

    def main(self):
        self.window.show()
        gtk.main()
        
if __name__=="__main__":
    iV = ImageViewer("ImageViewer.glade")
    iV.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
