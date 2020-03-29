---
title: pygtk example ss2cb (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'ss2cb'

Functions in program: 
* `def all_files_under(path):`
* `def copy_image(f):`

Modules used in program: 
* `import sys`
* `import os`
* `import gtk`
* `import pygtk`

## python ss2cb

Python pygtk example: ss2cb

```python
#! /usr/bin/python
import pygtk
pygtk.require('2.0')
import gtk
import os
import sys

def copy_image(f):
	assert os.path.exists(f), "file does not exist"
	image = gtk.gdk.pixbuf_new_from_file(f)
	clipboard = gtk.clipboard_get()
	clipboard.set_image(image)
	clipboard.store()

def all_files_under(path):
	path = os.path.expanduser(path);
	cur_path = path
	for filename in os.listdir(path):
		yield os.path.join(cur_path, filename)

if len(sys.argv) < 2:
	file = max(all_files_under('~/screens/'), key=os.path.getctime)
else:
	file = sys.argv[1]

copy_image(file);
print('File: "' + file + '" Copied')



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
