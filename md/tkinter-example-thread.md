---
title: tkinter example thread (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'thread'

Functions in program: 
* `def PRINTTEST():`
* `def poll():`

Modules used in program: 
* `import Adafruit_DHT`
* `import tkinter.ttk`
* `import tkinter`
* `import string`
* `import RPi.GPIO as GPIO`
* `import time`

## python thread

Python tkinter example: thread

```python
#! python3
import time
import RPi.GPIO as GPIO
import string
import tkinter
import tkinter.ttk
import Adafruit_DHT
from tkinter import messagebox
from tkinter import *
from threading import Thread

root = Tk()
root.title('PiTEST')
root.configure(background='black')

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

sensor = Adafruit_DHT.DHT22
pin = 4

keep_polling = True
temperature = None
humidity = None

# Stick what should be run in a different thread in a function
def poll():
    global keep_polling
    global temperature
    global humidity
    
    while keep_polling:
        humidity, temperature = Adafruit_DHT.read_retry(sensor, pin)
        temperature = temperature * 9/5.0 + 32

def PRINTTEST():
    print(temperature, humidity)

TESTTEXT = Label(root,text="TESTING",fg="white",bg="black",font='Consolas 20 bold')
TESTTEXT.grid(row=1,column=1,sticky="W,S,E")

B1 = tkinter.Button(root,bd=5,text="TEST",bg="gray",fg="white",command=PRINTTEST,height=4,width=20)
B1.grid(row=2,column=1,sticky="N,S,E,W",padx=8,pady=8)

# Then give it to a new Thread to run
thread = Thread(target = poll)
thread.start()

root.mainloop()

thread.join()

GPIO.cleanup()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
