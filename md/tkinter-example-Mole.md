---
title: tkinter example Mole (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Mole'

Functions in program: 
* `def convert():`
* `def showResult(t):`
* `def reverse(a):`
* `def fromKelvin(b, value):`
* `def toKelvin(a, value):`
* `def factor(a):`
* `def onConvertFromTrigger(a):`
* `def isTemperatureToTemperature(cFrom, cTo):`

Modules used in program: 
* `import tkinter as tk`

## python Mole

Python tkinter example: Mole

```python
# for Python 2
#import Tkinter as tk
#import ttk as ttk

# for Python 3
import tkinter as tk
from tkinter import ttk

# Configure
app = tk.Tk()
app.title('Chemistry Utility')

# Convert From
convertFromLabel = tk.Label(app,
                            text="Convert From")
convertFromLabel.grid(column=0, row=0)

convertFrom = ttk.Combobox(app,
                           values=[
                               "mol",
                               "cm^3",
                               "dm^3",
                               "g",
                               "molecules / atoms",
                               "gallon",
                               "quarts",
                               "Celcius",
                               "Fahrenheit",
                               "K"
                           ])
convertFrom.grid(column=0, row=1)

# Convert To
convertToLabel = tk.Label(app,
                          text="To")
convertToLabel.grid(column=0, row=2)
convertTo = ttk.Combobox(app,
                         values=[
                             "mol",
                             "cm^3",
                             "dm^3",
                             "g",
                             "molecules / atoms",
                             "gallon",
                             "quarts",
                             "Celcius",
                             "Fahrenheit",
                             "K"
                         ])
convertTo.grid(column=0, row=3)

massLabel = tk.Label(app,
                     text="Molar / Atomic Mass (g.)")
massLabel.grid(column=0, row=4)
mass = tk.Entry(app)
mass.grid(column=0, row=5)

quantityLabel = tk.Label(app,
                         text="Quantity")
quantityLabel.grid(column=0, row=6)
quantity = tk.Entry(app)
quantity.grid(column=0, row=7)

# Feedback
result = tk.Label(app)

# Core Logic
temperatureUnits = ['Celcius', 'Fahrenheit', 'K']
MAX_DECIMAL_PLACES = 5


def isTemperatureToTemperature(cFrom, cTo):
    return cFrom in temperatureUnits and cTo in temperatureUnits


def onConvertFromTrigger(a):
    _convertFrom = convertFrom.get()
    quantityLabel.config(text="Quantity (" + _convertFrom + ")")


def factor(a):
    if (a == 'mol'):
        return 1
    if (a == 'dm^3'):
        return 1.00/22.4
    if (a == 'cm^3'):
        return 0.001/22.4
    if (a == 'molecules / atoms'):
        return 1/(6.02*(10**23))
    if (a == 'g'):
        return 1/float(mass.get())
    # 1 GALLON = 3.78 LITRE -> 22.4 LITRE = 1 MOL
    if (a == 'gallon'):
        return 3.78/22.4
    # 1 QUART = 0.25 GALLON -> 1 GALLON = 3.78 LITRE -> 22.4 LITRE = 1 MOL
    if (a == 'quarts'):
        return 3.78 * 0.25 * 3.78 / 22.4
    return 1


def toKelvin(a, value):
    if (a == 'Celcius'):
        return value + 273.15
    if (a == 'Fahrenheit'):
        return (value - 32.00)*(5.00/9.00) + 273.15
    if (a == 'K'):
        return value


def fromKelvin(b, value):
    if (b == 'Celcius'):
        return value - 273.15
    if (b == 'Fahrenheit'):
        return (value - 273.15)*(9.00/5.00) + 32.00
    if (b == 'K'):
        return value


def reverse(a):
    return 1/a


def showResult(t):
    result.config(text=t)
    result.grid(row=9, column=0)


def convert():
    _convertFrom = convertFrom.get()
    _convertTo = convertTo.get()
    _qty = quantity.get()
    if (not _qty) or (not _convertFrom) or (not _convertTo):
        showResult("Nothing to convert!")
        return
    if (_convertFrom == 'g') or (_convertTo == 'g'):
        if not mass.get():
            showResult("'Molar / Atomic Mass' is required!")
            return
    if (_convertFrom in temperatureUnits) or (_convertTo in temperatureUnits):
        if not isTemperatureToTemperature(_convertFrom, _convertTo):
            showResult("Could not convert between quantity and temperature!")
            return
        else:
            _qty = float(_qty)
            # [IN] -> Kelvin
            _normalizeTemperature = toKelvin(_convertFrom, _qty)
            # Kelvin -> [OUT]
            _out = round(fromKelvin(
                _convertTo, _normalizeTemperature), MAX_DECIMAL_PLACES)
            showResult(str(_out) + " " + _convertTo)
            return
    else:
        _qty = float(_qty)
        _normalizeFrom = factor(_convertFrom) * _qty
        _out = round(_normalizeFrom * reverse(factor(_convertTo)),
                     MAX_DECIMAL_PLACES)
        showResult(str(_out) + " " + _convertTo)
        return

# Button
go = tk.Button(app, text="Convert", command=convert)
go.grid(row=8, column=0)

# Run
convertFrom.bind("<<ComboboxSelected>>", onConvertFromTrigger)
app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
