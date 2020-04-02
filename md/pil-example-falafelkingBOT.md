---
title: pil example falafelkingBOT (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'falafelkingBOT'

Functions in program: 
* `def main():`
* `def play():`
* `def leftClickfast():`
* `def leftClick():`
* `def screen_grab():`
* `def start_game():`
* `def fill():`
* `def spray():`
* `def give_it(rounds):`
* `def pita_filler():`
* `def salad_count(numimg):`
* `def get_salad():`
* `def chips_count( numimg):`
* `def get_chips():`
* `def falafel_count( numimg):`
* `def get_falafel():`
* `def hummus_count( numimg):`
* `def get_hummus():`

Modules used in program: 
* `import msvcrt`
* `import win32con`
* `import time`
* `import win32api`
* `import os`

## python falafelkingBOT

Python pil example: falafelkingBOT

```python
"""

FALAFEL KING BOT:

All coordinates assume a screen resolution of 1920x1080, and Chrome 
maximized with the Bookmarks Toolbar disabled.
Game link: http://www.net-games.co.il/media/girls/games/ae8308f387108abz.swf
x_pad = 309
y_pad = 65
Play area =  x_pad+1, y_pad+1, x_pad+1300, y_pad+975
Created by Yoav Levy.

"""
from PIL import Image
from PIL import ImageChops
from PIL import ImageGrab
import os
import win32api
import time
import win32con
import msvcrt
from PIL import ImageOps
from numpy import *

x_pad = 309
y_pad = 65


class Cord:
    f_hummus = ( 1115, 888)
    f_falafel = ( 754, 881)
    f_chips = ( 763, 742)
    f_salad = ( 1092, 748)

#hummus-----------------------------------------------------------

def get_hummus():
    box = ( 943, 786, 965, 815)
    im = ImageGrab.grab( box)
    return im

def hummus_count( numimg):
    im1 = Image.open( os.getcwd() + "\imgs\hummus\\1.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 1
    im1 = Image.open( os.getcwd() + "\imgs\hummus\\2.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 2
    im1 = Image.open( os.getcwd() + '\imgs\hummus\\3.png' )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 3
    else:
        return 0
#falafel-----------------------------------------------------------

def get_falafel():
    box = ( 586, 774, 611, 803)
    im = ImageGrab.grab( box)
    return im

def falafel_count( numimg):
    im1 = Image.open( os.getcwd() + "\imgs\\falafel\\1.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 1
    im1 = Image.open( os.getcwd() + "\imgs\\falafel\\2.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 2
    im1 = Image.open( os.getcwd() + "\imgs\\falafel\\3.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 3
    else:
        return 0
#chips-----------------------------------------------------------

def get_chips():
    box = ( 635, 657, 660, 685)
    im = ImageGrab.grab( box)
    return im

def chips_count( numimg):
    im1 = Image.open( os.getcwd() + "\imgs\\chips\\1.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 1
    im1 = Image.open( os.getcwd() + "\imgs\\chips\\2.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 2
    im1 = Image.open( os.getcwd() + "\imgs\\chips\\3.png" )
    diff = ImageChops.difference( im1, numimg).getbbox()
    if diff == None:
        return 3
    else:
        return 0
#salad-----------------------------------------------------------

def get_salad():
    box = (921, 682, 947, 708)
    im = ImageGrab.grab( box )
    return im

def salad_count(numimg):
    im1 = Image.open( os.getcwd() + "\imgs\\salad\\1.png" )
    diff = ImageChops.difference( im1, numimg ).getbbox()
    if diff == None:
        return 1
    im1 = Image.open( os.getcwd() + "\imgs\\salad\\2.png" )
    diff = ImageChops.difference( im1, numimg ).getbbox()
    if diff == None:
        return 2
    im1 = Image.open( os.getcwd() + "\imgs\\salad\\3.png" )
    diff = ImageChops.difference( im1, numimg ).getbbox()
    if diff == None:
        return 3
    else:
        return 0
#pita filler :D   ----------------------------------------------

def pita_filler():
    win32api.SetCursorPos(( 1398, 747))
    leftClick()

    win32api.SetCursorPos( Cord.f_hummus)
    x = hummus_count( get_hummus())
    for y in range( 0, x):
        leftClick()

    x=falafel_count(get_falafel())
    win32api.SetCursorPos( Cord.f_falafel)
    for y in range ( 0, x):
        leftClick()

    x=chips_count(get_chips())
    win32api.SetCursorPos( Cord.f_chips)
    for y in range ( 0, x):
        leftClick()

    x=salad_count(get_salad())
    win32api.SetCursorPos( Cord.f_salad)
    for y in range ( 0, x):
        leftClick()

def give_it(rounds):

    if rounds%2 != 0:
        win32api.SetCursorPos( (744, 531))
        leftClickfast()
        win32api.SetCursorPos( (1003, 520))
        leftClickfast()
        win32api.SetCursorPos( (1208, 544))
        leftClickfast()
        win32api.SetCursorPos( (1432, 567))
        leftClickfast()
        win32api.SetCursorPos( (1016, 324))
        leftClickfast()

    if rounds%2 == 0:

        win32api.SetCursorPos( (1016, 324))
        leftClickfast()
        win32api.SetCursorPos( (1432, 567))
        leftClickfast()
        win32api.SetCursorPos( (1208, 544))
        leftClickfast()
        win32api.SetCursorPos( (1003, 520))
        leftClickfast()
        win32api.SetCursorPos( (744, 531))
        leftClickfast()


def spray():
    win32api.SetCursorPos( (1265, 705))
    leftClick()


def fill():
    win32api.SetCursorPos( (484, 655))
    leftClick()

    for x in range( 0, 4):
        win32api.SetCursorPos( (911, 518))
        leftClickfast()
        win32api.SetCursorPos( (844, 568))
        leftClickfast()
        win32api.SetCursorPos( (768, 602))
        leftClickfast()
        win32api.SetCursorPos( (681, 648))
        leftClickfast()
    win32api.SetCursorPos( (509, 648))
    leftClick()


def start_game():
    win32api.SetCursorPos( (1503, 917))
    leftClick()
    win32api.SetCursorPos( (1393, 884))
    leftClick()
    win32api.SetCursorPos( (934, 423))
    leftClick()
    win32api.keybd_event( 0x42, 0, 0, 0)
    win32api.keybd_event( 0x4F, 0, 0, 0)
    win32api.keybd_event( 0x54, 0, 0, 0)
    win32api.keybd_event( 0xBD, 0, 0, 0)
    win32api.keybd_event( 0x59, 0, 0, 0)
    win32api.keybd_event( 0x4F, 0, 0, 0)
    win32api.keybd_event( 0x41, 0, 0, 0)
    win32api.keybd_event( 0x56, 0, 0, 0)
    win32api.SetCursorPos( (931, 552))
    leftClick()

def screen_grab():
    box = ( x_pad+1, y_pad+1, x_pad+1300, y_pad+975)
    im = ImageGrab.grab(box)
    #im.save( os.getcwd() + '\\full_snap__' + str( int( time.time())) + '.png', 'PNG')
    return im

def leftClick():
    win32api.mouse_event( win32con.MOUSEEVENTF_LEFTDOWN, 0, 0)
    time.sleep(0.8)
    win32api.mouse_event( win32con.MOUSEEVENTF_LEFTUP, 0, 0)
    print("Click."          #completely optional. But nice for debugging purposes.)

def leftClickfast():
    win32api.mouse_event( win32con.MOUSEEVENTF_LEFTDOWN, 0, 0)
    time.sleep( 0.3 )
    win32api.mouse_event( win32con.MOUSEEVENTF_LEFTUP, 0, 0)
    print("Click.")

def play():
        rounds = 1
        rounds = 1 + rounds
        pita_filler()
        give_it(rounds)
        fill()
        spray()
        time.sleep(1)
        screen_grab()


def main():
    start_game()
    while True:
        play()
     #   if msvcrt.kbhit() == True:
     #       key = msvcrt.getwch()
     #       if str(key) == '1':
     #           break
     #       else:
     #          continue
     #   else:
     #       continue

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
