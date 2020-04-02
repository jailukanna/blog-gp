---
title: pil example primes (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'primes'

Functions in program: 
* `def calcsieve():`
* `def plot(num):`
* `def convert(num):`
* `def calcsieve():`
* `def plot(num):`
* `def convert(num):`

Modules used in program: 
* `import sys`
* `import sys`

## python primes

Python pil example: primes

```python
#Made by Shaan Sheikh
#Stuyvesant HS
#February 2013
 
from PIL import Image
#Preview to what output should look like: http://imgur.com/gallery/0gI8eK6
#usage from terminal: $ python sieve.py width height
#       example used to generate linked preview: $ python sieve.py 683 384
#relies on PIL (Python Imaging Library) to function
 
from PIL import Image
import sys
 
try:
    width = int(sys.argv[1])
    height = int(sys.argv[2])
except IndexError:
    width = 1080
    height = 768
except ValueError:
    width = 1080
    height = 768
 
def convert(num):
    x = (num-1)%width
    y = (num-1)/width
   
    return (x,y)
 
def plot(num):
    tup = convert(num)
    x = tup[0]
    y = tup[1]
    pixdata[x,y] = (0,0,0)
 
def calcsieve():
    plot(1)
 
    print("eliminating multiples of 2")
    for m in range(4,width*height+1,2):
        plot(m)
        #print("\tkilling " + str(m))
    for a in range(3,int((width*height)**.5)+1,1):
        coor = convert(a)
        if (pixdata[coor[0],coor[1]][0]==255):
            print("eliminating multiples of " + str(a))
            for b in range(3*a,width*height+1,2*a):
                #print("\tkilling " + str(b))
                plot(b)
 
img = Image.new("RGB", (width,height), (255,255,255))
pixdata = img.load()
calcsieve()
img.save("sieve.png", "PNG")
import sys
 
try:
    width = int(sys.argv[1])
    height = int(sys.argv[2])
except IndexError:
    width = 1080
    height = 768
except ValueError:
    width = 1080
    height = 768
 
def convert(num):
    x = (num-1)%width
    y = (num-1)/width
   
    return (x,y)
 
def plot(num):
    tup = convert(num)
    x = tup[0]
    y = tup[1]
    pixdata[x,y] = (0,0,0)
 
def calcsieve():
    plot(1)
 
    print("eliminating multiples of 2")
    for m in range(4,width*height+1,2):
        plot(m)
        #print("\tkilling " + str(m))
    for a in range(3,int((width*height)**.5)+1,1):
        coor = convert(a)
        if (pixdata[coor[0],coor[1]][0]==255):
            print("eliminating multiples of " + str(a))
            for b in range(3*a,width*height+1,2*a):
                #print("\tkilling " + str(b))
                plot(b)
 
img = Image.new("RGB", (width,height), (255,255,255))
pixdata = img.load()
calcsieve()
img.save("sieve.png", "PNG")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
