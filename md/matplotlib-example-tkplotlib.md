---
title: matplotlib example tkplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'tkplotlib'


Modules used in program: 
* `import tkinter as tk`
* `import random`
* `import time`

## python tkplotlib

Python matplotlib example: tkplotlib

```python
import time
import random
import tkinter as tk
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from matplotlib.figure import Figure

class Window():
    def __init__(self, root):

        self.root = root

        self.root.protocol("WM_DELETE_WINDOW", self.quit)

        self.frame = tk.Frame(self.root)

        self.frame.pack(expand=True)

        button = tk.Button(master=self.frame, command=self.new_fig)
        button.pack(side='bottom')

        self.fig = None

        self.new_fig()


    def new_fig(self):

        if self.fig:
            self.fig.clear()
        else:
            self.fig = Figure()

            canvas = FigureCanvasTkAgg(self.fig, master=self.frame)
            self.widget = canvas.get_tk_widget()
            self.widget.pack(side="top", fill='both', expand=True)

        size = 10000

        data = [[random.gauss(1, 1) for _ in range(size)] for _ in range(size)]
        ax = self.fig.add_subplot(111)
        ax.imshow(data)
        self.fig.canvas.draw()

    def quit(self):
        self.root.quit()
        self.root.destroy()


if __name__ == '__main__':

    root = tk.Tk()
    w = Window(root)
    root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
