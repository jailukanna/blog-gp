---
title: tkinter example getMapping (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'getMapping'

Functions in program: 
* `def close(event):`
* `def update():`
* `def check():`
* `def fetch(file, value):`
* `def main():`

Modules used in program: 
* `import win32clipboard`
* `import csv`
* `import sys`

## python getMapping

Python tkinter example: getMapping

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import sys
import csv
import win32clipboard

if sys.version_info[0] < 3:
	import Tkinter as tk
else:
	import tkinter as tk

root = tk.Tk()
text = tk.Label(root, font=('times', 20, 'bold'))
data_old = ""

def main():
	root.bind_all('<Escape>', close)
	root.call('wm', 'attributes', '.', '-topmost', '1')
	text.pack(expand=1)
	update()
	root.mainloop()

def fetch(file, value):
	table = csv.reader(open(file))
	for row in table:
		if row[0] == value:
			return row[1]
	return "Unknown"

def check():
	global data_old
	win32clipboard.OpenClipboard()
	data = win32clipboard.GetClipboardData(win32clipboard.CF_UNICODETEXT)
	win32clipboard.CloseClipboard()
	if data == data_old:
		return False
	data_old = data
	if data.startswith("func_"):
		return fetch("./methods.csv", data)
	elif data.startswith("field_"):
		return fetch("./fields.csv", data)
	elif data.startswith("p_"):
		return fetch("./params.csv", data)
	else:
		return "Unknown"

def update():
	mcp = check()
	if mcp:
		text.config(text=mcp)
	root.after(1000, update)

def close(event):
	root.destroy()

if __name__ == "__main__":
	main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
