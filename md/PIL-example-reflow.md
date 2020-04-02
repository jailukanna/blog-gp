---
title: PIL example reflow (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'reflow'

Functions in program: 
* `def drawFlow():`
* `def keyChecker(event):`
* `def onMouseClick(event):`

## python reflow

Python PIL example: reflow

```python
try: #imports needed modules
    import PIL
    from PIL import Image,ImageGrab
    import pythoncom, pyHook
    import os
    import glob
except: 
    print(('Something went wrong with importing modules.'))

hm = pyHook.HookManager() #Hook manager assigned to hm for readability
iterator = 0 
toggle = False 
draw = False 
mousepos = None
capx = None
capy = None
capbox = None

###



def onMouseClick(event):
    if toggle == False: 
        print(('Capture is off'))
        return True #mouse stops working if this isn't true (???)
    else: 
        global iterator
        global mousepos
        global capx
        global capy
        global capbox
        iterator = iterator + 1
        mousepos = event.Position
        capx, capy = mousepos
        capbox = (capx - 200),(capy - 200),(capx + 200),(capy + 200)
        print((event.MessageName,str(iterator),event.Position))
        PIL.ImageGrab.grab(bbox=capbox).save("screencache" + str(iterator) + ".jpg" ) 
        return True

#Key toggle function
def keyChecker(event):
    global toggle
    global draw
    if event.Key == 'Oem_3':
        toggle = not toggle
        if toggle == True:
            print(('Capture is toggled on'))
        else:
            print(('Capture is toggled off'))
        return True
    elif event.Key == 'F':
        draw = not draw
        print(('Draw is on'))
        return True
    elif event.Key == 'Escape':
        exit()
    else: print(('Ignoring keystroke:',event.Key))
    return False 
    
#Compiles images and draws them to flowchart
def drawFlow():
    
    global draw
    if draw == False:
        return False
    else: return None
        
###

#Program

hm.MouseLeftDown = onMouseClick #when LMB is clicked run onMouseClick
hm.KeyDown = keyChecker #pass onto function keyChecker if any key is down
hm.HookMouse() #initiate mouse hooking
hm.HookKeyboard() #initiate keyboard hooking
pythoncom.PumpMessages() #wait for user input




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
