---
title: tkinter example visualizer (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'visualizer'


Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`
* `import time`
* `import sys`

## python visualizer

Python tkinter example: visualizer

```python
# Use python 3.4 + Tk 8.5.18
# Usage: $ python visualizer.py IN_FILE OUT_FILE
import sys
import time
import tkinter as tk
import tkinter.ttk as ttk

class Window(ttk.Frame):
	def __init__(self, master=None):
		super().__init__(master)
		self.canvas = tk.Canvas(self, bg = 'green', width=25*30, height=15*30)
		self.txt = tk.StringVar()
		self.Label = ttk.Label(self, textvariable=self.txt)
		self.skip = tk.DoubleVar()
		self.skip.set(1)

		scale = ttk.Scale(self, from_=0, to=100, variable=self.skip)
		drawButton = ttk.Button(self, text="Start", command=self.update_canvas)
		quitButton = ttk.Button(self, text="Quit", command=self.quit)
		self.canvas.pack()
		self.Label.pack()
		drawButton.pack()
		scale.pack()
		quitButton.pack()
		self.pack()

		self.field = []
		self.ans = []
		self.ans_i = 0;

	def update_canvas(self):
		self.canvas.delete("all")
		for step in range(int(self.skip.get())):
			self.field[int(self.ans[self.ans_i][0])-1][int(self.ans[self.ans_i][1])-1] -= 1
			self.ans_i += 1
			if len(self.ans) <= self.ans_i:
				break

		for i in range(30):
			for j in range(30):
				fill = 'black'
				if self.ans_i < len(self.ans) and i+1 == int(self.ans[self.ans_i][0]) and j+1 == int(self.ans[self.ans_i][1]):
					fill = 'red'
					
				backfill = '#%02xffff' % int((100-self.field[i][j])*255/100)
				self.canvas.create_rectangle(25*j+2, 15*i+2, 25*j+25+2, 15*i+15+2, fill=backfill, width=0)
				self.canvas.create_text(25*j+10, 15*i+10, text=str(self.field[i][j]), fill=fill)
		self.txt.set("Count : %d / %d" % (self.ans_i, len(self.ans)))
		self.canvas.update()

		if len(self.ans) <= self.ans_i :
			self.master.after_cancel(self.job)
			return

		time.sleep(0.001)
		self.job = self.master.after(0, self.update_canvas)


if __name__ == '__main__':
	param = sys.argv

	field = []
	with open(param[1], "r") as file:
		for line in file.readlines():
			ls = line.rstrip().split(' ')
			numlist = list(map(int, ls))
			field.append(numlist)

	ans = []
	with open(param[2], "r") as file:
		for line in file.readlines():
			ls = line.rstrip().split(' ')
			numlist = list(map(int, ls))
			ans.append(numlist)

	window = Window()
	window.field = field
	window.ans = ans
	window.master.title("daicon Visualizer")
	window.master.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
