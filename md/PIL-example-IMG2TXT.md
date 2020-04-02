---
title: PIL example IMG2TXT (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'IMG2TXT'

Functions in program: 
* `def isint(s):`
* `def getin(s, numerical = False):`
* `def convert(filename, x):`

## python IMG2TXT

Python PIL example: IMG2TXT

```python
'''Converts Images to ASCII'''
# Written for Python 2.7
# This Script requires PIL. You can get it using 'pip install PIL'

try:
    from PIL import Image
except ImportError:
    print("ERROR: Couldn't import PIL (Python Imaging Library). Get it using 'pip install PIL'")
    exit(0)
finally:
    try:
       import sys, os
    except ImportError:
        print("ERROR: Couldn't import SYS or OS. ")
        exit(0)

print("TEXT to ASCII converter written by GeeBeeKay")

class imgSize:
    def __init__(self,x,ratio):
        self.x = x
        self.y = int(x * ratio)
        self.r = ratio
        if self.y == 0:
            self.y = 1

def convert(filename, x):
    if not os.path.isfile(filename):
        print("ERROR: The file you tried to open doesn't exist!")
        return None

    chars = "$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/|()1}]?-+~>i!lI;:"+chr(34)+"^`'. "

    im = Image.open(filename).convert('LA')
    width = im.size[0]
    height = im.size[1]

    r = width/(height * 1.00)

    size = imgSize(x, r)

    if not width < size.x or not height < size.y:
        im = im.resize((size.x,size.y), Image.ANTIALIAS)
        width, height = im.size

    lines = []

    for y in range(height):
        line = ""
        for x in range(width):
            s = " "
            clr= im.getpixel((x, y))
            s = chars[int(clr[0] / 4)]
            line = line + s
        lines.append(line)

    f = open(filename + ".txt", "w")

    for line in lines:
        print(line)
        f.write(line + "\n")

    f.close()

def getin(s, numerical = False):
    o = ""
    while o == "":
        print(s)
        o = raw_input()
        if numerical:
            try:
                if int(o) == 0:
                   o = "1"
            except:
                print("ERROR: {} is not a number.".format(o))
                o = ""
    return o

def isint(s):
    try:
        int(s)
        return True
    except:
        return False

arg = sys.argv

if __name__ == "__main__":
    if not len(arg) < 3:
        print("Filename: {}, X-Scale: {}".format(arg[1], arg[2]))
        if isint(arg[2]):
            convert(arg[1], int(arg[2]))
        else:
            print("ERROR: Invalid argument at X-Scale: {} is not a number!".format(arg[2]))
            print("-"*60)
            convert(getin("File: "), int(getin("X-Scale: ", True)))
    else:
        convert(getin("File: "), int(getin("X-Scale: ", True)))

else:
    convert(getin("File: "), int(getin("X-Scale: ", True)))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
