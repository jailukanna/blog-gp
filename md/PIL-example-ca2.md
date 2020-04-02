---
title: PIL example ca2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ca2'

Functions in program: 
* `def main():`
* `def pil_render(data,height,width,rule):`
* `def ca_data(height,width,dorandom,rulenum):`

Modules used in program: 
* `import json`
* `import getopt,sys`

## python ca2

Python PIL example: ca2

```python
#!/usr/bin/env python
"""
 A slight adaptation of http://code.activestate.com/recipes/343386-wolfram-style-cellular-automata/
 to be called on the command line like
 for r in $(seq 0 255); do echo $r; ./ca2.py -w 9 -h 10 -R $r; done
 and writing both an image and a textual representation
"""

"""
 CellularAutomata.py: Wolfram-style cellular automata in Python

 Options:
 -h #  Use a screen of height # for the simulation
 -w #  Use a screen of width # for the simulation
 -r      Use a random initial row (rather than the standard single 1 in the middle)
 -R #  Use rule # for the simulation

"""

import getopt,sys
from random import randint
import json

def ca_data(height,width,dorandom,rulenum):
    # Create the first row, either randomly, or with a single 1
    if dorandom:
        first_row = [randint(0,1) for i in range(width)]
    else:
        first_row = [0]*width
        first_row[width/2] = 1
    results = [first_row]

    # Convert the rule number to a list of outcomes. 
    rule = [(rulenum/pow(2,i)) % 2 for i in range(8)]

    for i in range(height-1):
        data = results[-1]
        # Determine the new row based on the old data. We first make an
        #  integer out of the value of the old row and its two neighbors
        #  and then use that value to get the outcome from the table we
        #  computed earlier
        new = [rule[4*data[(j-1)%width]+2*data[j]+data[(j+1)%width]]
               for j in range(width)]
        #print(new)
        results.append(new)
    return results

def pil_render(data,height,width,rule):
    fname = str(rule) + ".png"
    from PIL import Image, ImageDraw
    img = Image.new("RGB",(width*10,height*10),(255,255,255))
    draw = ImageDraw.Draw(img)

    for y in range(height):
        for x in range(width):
            #if data[y][x]: draw.point((x*10,y*10),(0,0,0))
            if data[y][x]: draw.rectangle(((x*10,y*10),(x*10+10,y*10+10)), fill="black")
    img.save(fname,"PNG")
    return

def main():
    opts,args = getopt.getopt(sys.argv[1:],'h:w:rR:')
    height = 500
    width = 500
    dorandom = 0
    rule = 22
    for key,val in opts:
        if key == '-h': height = int(val)
        if key == '-w': width = int(val)
        if key == '-r': dorandom = 1
        if key == '-R': rule = int(val)
    data = ca_data(height,width,dorandom,rule)
    #print(data)
    #print(json.dumps(data))
    fn2 = str(rule) + ".txt"
    fh = open(fn2, 'w+')
    fh.write(json.dumps(data))
    fh.close()
    pil_render(data,height,width,rule)
    return

if __name__ == '__main__': main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
