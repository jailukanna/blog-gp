---
title: tkinter example cozad (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'cozad'

Functions in program: 
* `def calculate_hic():`

Modules used in program: 
* `import tkinter.font as font`
* `import tkinter as tk`

## python cozad

Python tkinter example: cozad

```python
import tkinter as tk
import tkinter.font as font

def calculate_hic():
    l1['text'] = "1"

root = tk.Tk()
frame = tk.Frame(root)
frame.pack()

helv36 = font.Font(family = "Helvetica", size = 36, weight = 'bold')

l1 = tk.Label(root, fg = 'blue', text = 'HIC', font = helv36)
l1.pack(side = tk.BOTTOM)

button = tk.Button(root, text = "Calculate HIC", command = calculate_hic, activebackground = 'orange', relief = tk.RAISED, borderwidth = 5, font = helv36)
button.pack(side = tk.TOP)

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
