---
title: tkinter example auto (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'auto'

Functions in program: 
* `def display_total_charges():`

Modules used in program: 
* `import Tkinter # or ... import tkinter`
* `import tkMessageBox # or ... import tkinter.messagebox`

## python auto

Python tkinter example: auto

```python
# prompt:
#   Joe's Automotive performs the following routine maintenance services:
#     + Oil change - $30.00
#     + Lube job - $20.00
#     + Radiator flush - $40.00
#     + Transmission flush - $100.00
#     + Inspection - $35.00
#     + Muffler replacement - $200.00
#     + Tire rotation - $20.00
#   Write a GUI program with check buttons that allow the user to select any or all of these services.
#   When the user clicks a button, the total charges should be displayed.

# resources:
#   + https://www.tutorialspoint.com/python/python_gui_programming.htm
#   + https://www.tutorialspoint.com/python/tk_checkbutton.htm
#   + http://stackoverflow.com/a/6469789/670433

#
# import modules:
#

from Tkinter import * # or ... from tkinter import *
import tkMessageBox # or ... import tkinter.messagebox
import Tkinter # or ... import tkinter

#
# prepare/define GUI window:
#

window = Tkinter.Tk() # or ... window = tkinter.Tk()

#
# define variables that will hold the checked/unchecked values (onvalue/offvalue):
#

oil_checked = IntVar()
lube_checked = IntVar()
radiator_checked = IntVar()
transmission_checked = IntVar()
inspection_checked = IntVar()
muffler_checked = IntVar()
tire_checked = IntVar()

#
# define the function which will be invoked when any checkbox is toggled on/off:
#

def display_total_charges():
    total_charges = oil_checked.get() + lube_checked.get() + radiator_checked.get() + transmission_checked.get() + inspection_checked.get() + muffler_checked.get() + tire_checked.get()
    print("")
    print("TOTAL CHARGES: " + str(total_charges) )

#
# define individual checkbox elements, supplying parameters corresponding to:
#  + the display text ("text"),
#  + the function to invoke when toggled ("command"),
#  + the variable to store the toggled value ("variable")
#  + the dollar value corresponding with the service charge ("onvalue"):
#

oil_button = Checkbutton(window, text = "Oil Change",                   variable = oil_checked,             onvalue = 30,  offvalue = 0, command = display_total_charges, height = 5, width = 20)
lube_button = Checkbutton(window, text = "Lube Job",                    variable = lube_checked,            onvalue = 20,  offvalue = 0, command = display_total_charges, height = 5, width = 20)
radiator_button = Checkbutton(window, text = "Radiator Flush",          variable = radiator_checked,        onvalue = 40,  offvalue = 0, command = display_total_charges, height = 5, width = 20)
transmission_button = Checkbutton(window, text = "Transmission Flush",  variable = transmission_checked,    onvalue = 100, offvalue = 0, command = display_total_charges, height = 5, width = 20)
inspection_button = Checkbutton(window, text = "Inspection",            variable = inspection_checked,      onvalue = 35,  offvalue = 0, command = display_total_charges, height = 5, width = 20)
muffler_button = Checkbutton(window, text = "Muffler Replacement",      variable = muffler_checked,         onvalue = 200, offvalue = 0, command = display_total_charges, height = 5, width = 20)
tire_button = Checkbutton(window, text = "Tire Rotation",               variable = tire_checked,            onvalue = 20,  offvalue = 0, command = display_total_charges, height = 5, width = 20)

#
# prepare/build/bind elements to the GUI window:
#

oil_button.pack()
lube_button.pack()
radiator_button.pack()
transmission_button.pack()
inspection_button.pack()
muffler_button.pack()
tire_button.pack()

#
# open/invoke the GUI window:
#

window.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
