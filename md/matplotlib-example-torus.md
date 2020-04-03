---
title: matplotlib example torus (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'torus'


Modules used in program: 
* `import matplotlib.cm as cm`
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`

## python torus

Python matplotlib example: torus

```python
"""
Game of Life on a torus

Uses an amazingly concise pair of lines that do the update.
They are nearly identical to Allen Downey's code from
http://greenteapress.com/complexity/Life.py

The torus part is in mode='wrap'.
"""

from scipy.ndimage import convolve
import numpy as np
import matplotlib
matplotlib.use('TkAgg')
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import matplotlib.cm as cm


class Board:
	def __init__(self,boardx=50,boardy=50):
		self.filter = np.array([[1,1,1],[1,10,1],[1,1,1]],dtype='float')
		self.board = np.zeros((boardx,boardy),dtype=bool)	
		self.add_glider(x=0,y=boardy/2,orientation='se')
		self.add_glider(x=boardx/2,y=0,orientation='sw')
		self.add_glider(x=3*boardx/4,y=boardy-1,orientation='nw')
		self.add_glider(x=0,y=boardy-1,orientation='ne')
		self.fig1 = plt.figure()
	
	def animation(self):
		return animation.FuncAnimation(self.fig1, self.update , frames=500,init_func=self.image, interval=50, blit=True, repeat_delay=1000)

	def add_glider(self, x=0, y=0,orientation='se'): 
		assert orientation in ('se','sw','ne','nw')
		for i, j in  [(0,1), (1,2), (2,0), (2,1), (2,2)]: 
			if orientation == 'se':
				pass
			elif orientation =='sw':
				i = -i
			elif orientation == 'ne':
				j = -j
			elif orientation == 'nw':
				i,j = -i,-j

			self.board[x+i, y+j] = 1.0
	def image(self):
		self.im = plt.imshow(1.0-board.board,cmap=cm.Greys_r)
		return (self.im, )
	def update(self,*y):
		conv = convolve(self.board,self.filter,mode='wrap')
		self.board = (conv == 12) | (conv == 13) | (conv == 3)
		self.im.set_array(1.0-self.board)
		return (self.im, )

if __name__ == "__main__":
	board = Board()
	ani = board.animation()
	plt.show()








```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
