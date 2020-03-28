---
title: tkinter example tkinter filename getter (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkinter filename getter'

Functions in program: 
* `def fileopen ():`

## python tkinter filename getter

Python tkinter example: tkinter filename getter

```python
def fileopen ():
	import tkinter
	import tkinter.filedialog
	tk = tkinter.Tk()
	tk.withdraw()
	args = {"initialdir" : "c:", "filetypes" : [("テキストファイル", "*.txt")], "title": "ファイル選択"}
	name = tkinter.filedialog.askopenfilename(**args)
	return name


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
