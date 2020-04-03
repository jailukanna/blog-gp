---
title: matplotlib example quitar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'quitar'


Modules used in program: 
* `import tkinter as tk `

## python quitar

Python matplotlib example: quitar

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

##. tkinter
import tkinter as tk 

root= tk.Tk() 
canvas1 = tk.Canvas(root, width = 350, height = 250) 
canvas1.pack()
button1 = tk.Button (root, text='Quitar ventana', command=root.destroy) 
canvas1.create_window(170, 130, window=button1)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
