---
title: turtle example hello (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'hello'

Functions in program: 
* `def main():`
* `def main():`

## python hello

Python turtle example: hello

```python
#!/usr/bin/python

from turtle import *

def main():
	radius=100
	rate=0.65
	reset()
	up()
	goto(-300,200)
	width(radius*0.07)
	color("#1d531d","#3da639")
	right(90)
	forward(radius*0.5)
	left(90)
	circle(radius, 20)
	down()
	begin_fill()
	circle(radius, 320)
	left(90)
	forward(radius*rate)
	right(90)
	circle(radius*(1-rate),-320)
	right(90)
	forward(radius*rate)
	end_fill()
	up()
	left(90)
	circle(radius,-20)
	right(90)
	forward(0.5*radius)
	write("Oops", False , align="center",font=("Arial", 20, "normal"))
	
	goto(0,0)
	width(radius*0.02)
	down()
	for x in range(1,100):
		color((x/100, x/100, x/100),"#3da639")
		begin_fill()
		forward(100-x);
		left(90)
		forward(100-x);
		left(90)
		forward(100-x);
		left(90)
		forward(100-x);
		left(10)
		end_fill()

	ht()

if __name__ == '__main__':
	main()
	mainloop()#!/usr/bin/python

from turtle import *

def main():
	radius=100
	rate=0.65
	reset()
	up()
	goto(-300,200)
	width(radius*0.07)
	color("#1d531d","#3da639")
	right(90)
	forward(radius*0.5)
	left(90)
	circle(radius, 20)
	down()
	begin_fill()
	circle(radius, 320)
	left(90)
	forward(radius*rate)
	right(90)
	circle(radius*(1-rate),-320)
	right(90)
	forward(radius*rate)
	end_fill()
	up()
	left(90)
	circle(radius,-20)
	right(90)
	forward(0.5*radius)
	write("Open Source Initiative", False , align="center",font=("Arial", 20, "normal"))
	
	goto(0,0)
	width(radius*0.02)
	down()
	for x in range(1,100):
		color((x/100, x/100, x/100),"#3da639")
		begin_fill()
		forward(100-x);
		left(90)
		forward(100-x);
		left(90)
		forward(100-x);
		left(90)
		forward(100-x);
		left(10)
		end_fill()

	ht()

if __name__ == '__main__':
	main()
	mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
