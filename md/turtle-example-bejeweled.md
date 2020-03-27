---
title: turtle example bejeweled (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'bejeweled'

Functions in program: 
* `def main():`
* `def get_mouse_position():`
* `def move_mouse(x, y):`
* `def click(x, y):`
* `def release_mouse(x, y, button = 1):`
* `def press_mouse(x, y, button = 1):`
* `def pos_to_screen(x, y):`
* `def vertausche(src, dest):`
* `def bewerte_board(board):`
* `def paint_suggestion(status, tauschvorschlaege):`
* `def find_suggestion(status):`
* `def right(status, x, y):`
* `def rightpos(x, y):`
* `def left(status, x, y):`
* `def leftpos(x, y):`
* `def bottom(status, x, y):`
* `def bottompos(x, y):`
* `def top(status, x, y):`
* `def toppos(x, y):`
* `def col_val(tup):`
* `def ReadBejeweledState():`

Modules used in program: 
* `import time`
* `import Image, ImageDraw`
* `import os`
* `import sys`

## python bejeweled

Python turtle example: bejeweled

```python
#!/usr/bin/env python
# encoding: utf-8
"""
bejeweled.py - simple bot to play Bejeweled 3 on a Mac for you

Created by Maximillian Dornseif on 2011-08-09.
"""

import sys
import os
from Quartz.CoreGraphics import *
from AppKit import *
from turtle import *
import Image, ImageDraw
from Quartz import CGPostMouseEvent, CGWarpMouseCursorPosition, CGDisplayPixelsHigh, CGDisplayPixelsWide
from AppKit import NSEvent

backgroundimage = None
windowpos = None
CELLSIZE = 82
CROPX = 334
CROPY = 70 # Normal Play
#CROPY = 105 # Lightning

def ReadBejeweledState():
    global backgroundimage, windowpos
    v = CGWindowListCopyWindowInfo(0, 0)
    for w in v:
        try:
            if w[kCGWindowName] == u'Bejeweled 3':
                windowpos = (w[kCGWindowBounds]['X'], w[kCGWindowBounds]['Y'])
                windowImage = CGWindowListCreateImage(CGRectNull,
                                                      kCGWindowListOptionIncludingWindow,
                                                      w[kCGWindowNumber],
                                                      kCGWindowImageBoundsIgnoreFraming)
                # bits = NSBitmapImageRep.alloc().initWithData_(windowImage)
                
                # Create a bitmap rep from the image...
                # NSBitmapImageRep *bitmapRep = [[NSBitmapImageRep alloc] initWithCGImage:cgImage];
                bitmapRep = NSBitmapImageRep.alloc().initWithCGImage_(windowImage)
                # Create an NSImage and add the bitmap rep to it...
                # NSImage *image = [[NSImage alloc] init];
                image = NSImage.alloc().init()
                # [image addRepresentation:bitmapRep];
                image.addRepresentation_(bitmapRep)
                #Height = 790;
                #Width = 1024;
                #X = 583;
                #Y = 114;

                im = Image.fromstring("RGBA", 
                                       (int(bitmapRep.pixelsWide()), 
                                        int(bitmapRep.pixelsHigh())),
                                        bitmapRep.bitmapData().tobytes(),
                                        "raw", 'ARGB')
                #tracer (False)    # This too: rendering the 'turtle' wastes time

                im = im.crop((CROPX, CROPY, CROPX+(CELLSIZE*8), CROPY+(CELLSIZE*8)))
                grid = {None: None}
                for x in range(8):
                    for y in range(8):
                        # wir bilden den schnitt aus 4 Pixeln
                        col1 = im.getpixel((x*CELLSIZE+CELLSIZE/2+4, y*CELLSIZE+CELLSIZE/2-4))
                        col2 = im.getpixel((x*CELLSIZE+CELLSIZE/2+4, y*CELLSIZE+CELLSIZE/2+4))
                        col3 = im.getpixel((x*CELLSIZE+CELLSIZE/2-4, y*CELLSIZE+CELLSIZE/2-4))
                        col4 = im.getpixel((x*CELLSIZE+CELLSIZE/2-4, y*CELLSIZE+CELLSIZE/2+4))
                        # im.putpixel((x*CELLSIZE+CELLSIZE/2, y*CELLSIZE+CELLSIZE/2), (255,255,255,0))
                        col = ((col1[0]+col2[0]+col3[0]+col4[0])/4,
                               (col1[1]+col2[1]+col3[1]+col4[1])/4,
                               (col1[2]+col2[2]+col3[2]+col4[2])/4,
                               0)
                        grid[x, y] = col_val(col)
                #im.show()
                backgroundimage = im
                return grid
        except KeyError:
            # window has no name - happens
            pass

def col_val(tup):
    """Wandelt einen Farb wert in einen Buchstaben um."""
    (r, g, b, a) = tup
    r = r/16
    g = g/16
    b = b/16
    # fa6, fb5, f94, e73, ea4, ea4, 
    if r >= 0xe and g in [7, 8, 9, 0xa, 0xb] and b <= 6:
        return "O"  # Orange
    if r > g+b and r >= 0xd:
        return "R"  # Red
    # ff5 ca1 c90
    if r >= 0xc and g >= 0x9 and b <= 5:
        return "Y"  # Yellow
    if r >= 0xe and g <= 6 and b >= 0xe:
        return "M"  # Magenta
    # 06d 37d
    if b >= 0xd and r + g <= b:
        return "B"  # Blue
    # bbb
    if r + g + b >= 33 and b > 0xa and g > 0xa:
        return "W"  # White
    if g > r + b:
        return "G"  # Green
    # fb5
    print("%x%x%x" % (r,g,b))
    return "%x%x%x" % (r,g,b)


def toppos(x, y):
    if y >= 1:
        return (x, y-1)
    return None
    
def top(status, x, y):
    "liefert den wert darueber"
    return status[toppos(x, y)]

def bottompos(x, y):
    if y < 7:
        return (x, y+1)
    return None

def bottom(status, x, y):
    "liefert den wert darunter"
    return status[bottompos(x, y)]

def leftpos(x, y):
    if x >= 1:
        return(x-1, y)
    return None


def left(status, x, y):
    "liefert den wert links davon"
    return status[leftpos(x, y)]

def rightpos(x, y):
    if x <= 6:
        return (x+1, y)
    return None
    

def right(status, x, y):
    "liefert den wert rechts davon"
    return status[rightpos(x, y)]


def find_suggestion(status):
    tauschvorschlaege = []
    for y in range(7, -1, -1):
        for x in range(8):
            # Mittlere vertauschung
            # wir suchen zunächst: X
            #                       X
            #                      X
            if top(status, x, y) \
               and (top(status, x, y) == bottom(status, x, y)) \
               and (top(status, x, y) == right(status, x, y)):
                tauschvorschlaege.append(((x,y), rightpos(x, y), status[x, y], right(status, x, y)))
            # nun:  X
            #      X
            #       X
            if top(status, x, y) \
               and (top(status, x, y) == bottom(status, x, y)) \
               and (top(status, x, y) == left(status, x, y)):
                tauschvorschlaege.append(((x,y), leftpos(x,y), status[x, y], left(status, x, y)))
            # nun:  X
            #      X X
            if top(status, x, y) \
               and (top(status, x, y) == left(status, x, y)) \
               and (top(status, x, y) == right(status, x, y)):
                tauschvorschlaege.append(((x,y), toppos(x,y), status[x, y], top(status, x, y)))
            # nun:  X
            #      X X
            if bottom(status, x, y) \
               and (bottom(status, x, y) == left(status, x, y)) \
               and (bottom(status, x, y) == right(status, x, y)):
                tauschvorschlaege.append(((x,y), bottompos(x,y), status[x, y], bottom(status, x, y)))
            # Vertauschung am Ende (unten)
            # Zwei gleiche Steine übereinander finden
            #  X
            #  X
            # A B
            #  C
            if top(status, x, y) \
                and top(status, x, y) == top(status, *toppos(x, y)):
                # Prüfen, ob darunter eine Vertauschung möglich ist.
                if (top(status, x, y) == right(status, x, y)):
                    tauschvorschlaege.append(((x,y), rightpos(x, y), status[x, y], right(status, x, y)))
                if (top(status, x, y) == left(status, x, y)):
                    tauschvorschlaege.append(((x,y), leftpos(x, y), status[x, y], left(status, x, y)))
                if (top(status, x, y) == bottom(status, x, y)):
                    tauschvorschlaege.append(((x,y), bottompos(x, y), status[x, y], bottom(status, x, y)))
            # Vertauschung am Anfang (oben)
            #  C
            # A B
            #  X
            #  X
            if bottom(status, x, y) \
                and bottom(status, x, y) == bottom(status, *bottompos(x, y)):
                # Prüfen, ob darüber eine Vertauschung möglich ist.
                if (bottom(status, x, y) == right(status, x, y)):
                    tauschvorschlaege.append(((x,y), rightpos(x, y), status[x, y], right(status, x, y)))
                if (bottom(status, x, y) == left(status, x, y)):
                    tauschvorschlaege.append(((x,y), leftpos(x, y), status[x, y], left(status, x, y)))
                if (bottom(status, x, y) == top(status, x, y)):
                    tauschvorschlaege.append(((x,y), toppos(x, y), status[x, y], top(status, x, y)))
            # Vertauschung rechts
            #   A
            # XX C 
            #   B
            if left(status, x, y) \
                and left(status, x, y) == left(status, *leftpos(x, y)):
                # Prüfen, ob eine Vertauschung möglich ist.
                if (left(status, x, y) == top(status, x, y)):
                    tauschvorschlaege.append(((x,y), toppos(x, y), status[x, y], top(status, x, y)))
                if (left(status, x, y) == bottom(status, x, y)):
                    tauschvorschlaege.append(((x,y), bottompos(x, y), status[x, y], bottom(status, x, y)))
                if (left(status, x, y) == right(status, x, y)):
                    tauschvorschlaege.append(((x,y), rightpos(x, y), status[x, y], right(status, x, y)))
            # Vertauschung links
            #   A
            #  C XX
            #   B
            if right(status, x, y) \
                and right(status, x, y) == right(status, *rightpos(x, y)):
                # Prüfen, ob eine Vertauschung möglich ist.
                if (right(status, x, y) == top(status, x, y)):
                    tauschvorschlaege.append(((x,y), toppos(x, y), status[x, y], top(status, x, y)))
                if (right(status, x, y) == bottom(status, x, y)):
                    tauschvorschlaege.append(((x,y), bottompos(x, y), status[x, y], bottom(status, x, y)))
                if (right(status, x, y) == left(status, x, y)):
                    tauschvorschlaege.append(((x,y), leftpos(x, y), status[x, y], left(status, x, y)))
                
            
    print(tauschvorschlaege)
    paint_suggestion(status, tauschvorschlaege)
    bestmove_score = 0
    bestmove = None
    print("Bewerte ...",)
    for src, dest, farbesrc, farbedest in tauschvorschlaege:
        newstatus = {}
        newstatus.update(status)
        newstatus[src], newstatus[dest] = status[dest], status[src]
        score = bewerte_board(newstatus)
        print(score,)
        if score > bestmove_score:
            bestmove = (src, dest, farbesrc, farbedest)
            bestmove_score = score
    print
    if bestmove:
        (src, dest, farbesrc, farbedest) = bestmove
        vertausche(src, dest)


fillmap = dict(R=(255, 0, 0, 255),
               G=(0, 255, 0, 128),
               B=(0, 0, 255, 128),
               W=(255, 255, 255, 128),
               M=(255, 0, 255, 128),
               Y=(255, 255, 0, 255),
               O=(255, 150, 50, 128),
               )

def paint_suggestion(status, tauschvorschlaege):
    im = backgroundimage
    if im:
        im = im.resize((240, 240))
        draw = ImageDraw.Draw(im)
        for y in range(8):
            for x in range(8):
                if status[x, y]:
                    draw.rectangle(((x*30+10, y*30+10), (x*30+20, y*30+20)), fill=fillmap.get(status[x, y], (0,0,0,255)))


        for src, dest, farbesrc, farbedest in tauschvorschlaege:
            sx, sy = src
            dx, dy = dest
            draw.line(((sx*30+15, sy*30+15), (dx*30+15, dy*30+15)), fill=(255,255,255), width=5)
        del draw 
        # im.show()
        im.save("helper.png", "PNG")
        import os
        # open image then bring game  to front
        os.system("open -F helper.png ; open '/Applications/Games/Bejeweled 3.app'")
        os.system("open '/Applications/Games/Bejeweled 3.app'")


def bewerte_board(board):
    "Bewertet das spielfeld (status) nach einem Vertauschungsvorgang."
    
    # findet alle Zeilen mit mehr als 2 Identischen Farben nebeneinander
    findsh = []
    for y in range(8):
        current = None
        current_len = 1
        for x in range(8):
            if board[x,y] == current:
                current_len += 1
            else:
                # Andersfarbiger Stein
                if current_len > 2:
                    findsh.append(current_len)
                current_len = 1
            current = board[x,y]
        # Ende der Zeile
        if current_len > 2:
            findsh.append(current_len)

    # findet alle Zeilen mit mehr als 2 Identischen Farben uebereinander
    findsv = []
    for x in range(8):
        current = None
        current_len = 1
        for y in range(8):
            if board[x,y] == current:
                current_len += 1
            else:
                # Andersfarbiger Stein
                if current_len > 2:
                    findsv.append(current_len)
                current_len = 1
            current = board[x,y]
        # Ende der Zeile
        if current_len > 2:
            findsv.append(current_len)

    # Score bestimmen
    score = 0
    # Für horizontale Zeilen berechnen wir pro laengeneinheit 11 Punkte
    for e in findsh:
        score += e * 11
    # Vertikale Zeilen haben wir nicht ganz so gerne pro laengeneinheit 10 Punkte
    for e in findsv:
        score += e * 10
    return score


def vertausche(src, dest):
    sx, sy = src
    dx, dy = dest
    pause = 0.05
    print( src, '<->', dest)
    #move_mouse(sx, sy)
    time.sleep(pause)
    click(sx, sy)
    #move_mouse(dx, dy)
    time.sleep(pause)
    click(dx, dy)
    # Maus aus dem weg
    # move_mouse(8, 8)
    print('done')

def pos_to_screen(x, y):
        xpos = x * CELLSIZE
        xpos += windowpos[0] + CROPX + (CELLSIZE/2)
        ypos = y * CELLSIZE
        ypos += windowpos[1] + CROPY + (CELLSIZE/2)
        return float(xpos), float(ypos)
        
def press_mouse(x, y, button = 1):
        xpos, ypos = pos_to_screen(x,y)
        button_list = [0, 0, 0]
        button_list[button - 1] = 1
        CGPostMouseEvent((xpos, ypos), 1, 3, *button_list)

def release_mouse(x, y, button = 1):
        xpos, ypos = pos_to_screen(x,y)
        CGPostMouseEvent((xpos, ypos), 1, 3, 0, 0, 0)

def click(x, y):
    mx, my = get_mouse_position()
    move_mouse(x, y)
    press_mouse(x, y)
    release_mouse(x, y)
    # restore mouse position
    CGWarpMouseCursorPosition((mx, my))


def move_mouse(x, y):
        xpos, ypos = pos_to_screen(x,y)
        CGWarpMouseCursorPosition((xpos, ypos))


def get_mouse_position():
        loc = NSEvent.mouseLocation()
        return loc.x, CGDisplayPixelsHigh(0) - loc.y

import time
laststate = None

def main():
    global laststate
    while True:
        status = ReadBejeweledState()
        for y in range(8):
            for x in range(8):
                print(status[x,y],)
            print
        print
        find_suggestion(status)
        sleeptime = 1
        if status == laststate:
            sleeptime = 5
        laststate = status
        if windowpos:
            mx, my = get_mouse_position()
            if mx < windowpos[0] or my < windowpos[1]:
                sleeptime = 5
            if my > windowpos[0]+1050 or my > windowpos[1]+800:
                sleeptime = 5
        time.sleep(sleeptime)
        
if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
