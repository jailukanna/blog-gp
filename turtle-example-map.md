---
title: turtle example map (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'map'

Functions in program: 
* `def getcoords(index):`
* `def tileinfo(index):`
* `def debuginfo():`
* `def gotostart(m):`
* `def drawmap(m):`
* `def move(m, lasttile):`
* `def draw_tile(tile):`
* `def main():`

## python map

Python turtle example: map

```python
from turtle import *

def main():
#Instruction for the main loop.
	home()
	delay(0)
	gotostart(map)
	debuginfo()
	drawmap(map)
	done()

#Define cardinal directions.
direct = {
	'NORTH': 90,
	'EAST': 0,
	'SOUTH': 270,
	'WEST': 180}

settings = {
	''
}

#Define tile size (tilees are squares).
tilesize = 50

#Define tile colors.
tileset = {
	'GRASS': 0,
	'WATER': 1,
	'SAND': 2,
	'ROCK': 3}

#Define our map to draw.
#map = [3,			#Set row width.
#		3, 0, 0,	#Row data.
#		0, 2, 1,
#		2, 1, 1]
map = [4,
		0, 0, 0, 0,	
		0, 1, 1, 0,
		0, 1, 1, 3,
		0, 2, 2, 2]

#Used to keep track of the last tile drawn.
lasttile = 0

def draw_tile(tile):
#Draws a singular tile.

	#Set color of tile
	if tile == tileset['GRASS']:
		color('brown', 'green')
	elif tile == tileset['WATER']:
		color('blue', 'aqua')
	elif tile == tileset['SAND']:
		color('brown', 'yellow')
	elif tile == tileset['ROCK']:
		color('gray', 'gray')
	else:
		unknown_tile()

	#Draw a tile.
	down()
	begin_fill()
	seth(direct['NORTH'])
	forward(tilesize)
	seth(direct['WEST'])
	forward(tilesize)
	seth(direct['SOUTH'])
	forward(tilesize)
	seth(direct['EAST'])
	forward(tilesize)
	end_fill()
	up()

def move(m, lasttile):
#Determines where to begin drawing the next tile.
	w = m[0]					#Make a copy of the map's width value.
	forward(tilesize)			#Always start by moving forward one tile.
	if lasttile < (w + 1):		#If the tile is in the first row...
		pass					#Do nothing.
	if lasttile % w == 0:		#If the last tile was the last in the row...
		right(90)				#Turn toward the bottom of the current tile.
		forward(tilesize)		#Go to the bottom of the current tile.
		right(90)				#Turn to the left of the current tile.
		forward(tilesize * w)	#Move all the way down te row.
		right(90)				#Get in position for next row.

def drawmap(m):
#Print the entire map.
	for x in range (0, len(m)):
		if x == 0:
			print("Reading map data....") #Not really, just skipping index 0.
		else:
			draw_tile(map[x])
			lasttile = x
			tileinfo(x)
			move(m, lasttile)
	print("Map drawn.")

def gotostart(m):
#Go to the top left of the map.
#Makes maps more or less centered.
	up()
	topleft = ((m[0] * tilesize) / 2) #Row width * tile size in px, halfed.
	setpos (-topleft, topleft)
	down()

def debuginfo():
#Numbers and stats that help me develop.
#Printed into sysout.
	print(("Animation delay: ", end=''))
	print((delay()))
	print(("Tile size: ", end=''))
	print((tilesize))
	print(("Tileset: ", end=''))
	print((tileset))
	print(("Map Width: " , end=''))
	print((map[0]))
	print(("Map Data: ", end=''))
	print((map[1:]))
	print(("Turtle Start Point: ", end=''))
	print((position(), end = ''))
	print((heading()))

def tileinfo(index):
#Print info on the last drawn tile.
	print(("Drew tile ", end=''))
	print((index, end=''))
	getcoords(index)
	print((" as color ", end=''))
	print((map[index]))

def getcoords(index):
#Determine and print(the coords of the last tile based on its index.)
	print((" (", end=''))
	if (index % map[0]) == 0:
		print(((index // map[0]), end=''))
	else:
		print(((index // map[0] + 1), end=''))
	print((",", end=''))
	if (index % map[0]) == 0:
		print((map[0], end=''))
	else:
		print((index % map[0], end=''))
	print((")", end=''))

#Run the main program loop.
main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
