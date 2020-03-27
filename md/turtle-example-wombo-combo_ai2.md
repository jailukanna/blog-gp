---
title: turtle example wombo-combo ai2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo ai2'

Functions in program: 
* `def tah(pole, pozice, symbol):`
* `def tah_pocitace(pole):`

Modules used in program: 
* `import random`

## python wombo-combo ai2

Python turtle example: wombo-combo ai2

```python
# def nacti_cislo():
#     odpoved = input("Zadej cislo: ")
#     try:
#         cislo = 11/int(odpoved)
#     except ZeroDivisionError:
#         cislo = 11/0.00000001
#         print("Deleni nulou, delim namisto toho infiminitezalnim cimsi.")
#     except ValueError:
#         cislo = 0
#         print("Deleni pismenem nejde, nastavim nulu")
#
#     print("Cislo je: {}".format(cislo))
#
# nacti_cislo()

#####    AI   #####

import random

def tah_pocitace(pole):
    pozice = random.randrange(len(pole))
    symbol = 'o'
    return tah(pole, pozice, symbol)

def tah(pole, pozice, symbol):
    return pole[:pozice] + symbol + pole[pozice+1:]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
