---
title: turtle example wombo-combo retezce (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo retezce'

Functions in program: 
* `def piskvorky1D(x):`
* `def tah_pocitace(pole):`
* `def tah_hrace(pole):`
* `def tah(pole, pozice, symbol):`
* `def vyhodnot(pole):`
* `def zamen (retezec, pozice, znak):`

## python wombo-combo retezce

Python turtle example: wombo-combo retezce

```python
# retezec="Caute, tady Vlada \n a zdravim vas!"
# print(retezec)
#
# print(r"C:\Program Files\Git\cmd\git.exe")
#
# print("2"+"4")
#
# retez = "cokolada"
# print(retez[3])
# print(retez[:3])
# print(retez[:-3])
# print(retez[1:3])
# print(retez[-3])
# print(retez.capitalize())
#

from random import randrange

def zamen (retezec, pozice, znak):
    novy_retezec = retezec[:pozice] + znak + retezec[pozice+1:]
    return novy_retezec

# print(zamen("Vladimir",3,"o"))

def vyhodnot(pole):
    if "xxx" in pole:
        print("Vyhral x!")
        return False
    elif "ooo"in pole:
        print("Vyhral o!")
        return False
    else:
        return True


def tah(pole, pozice, symbol):
    return pole[:pozice] + symbol + pole[pozice+1:]

def tah_hrace(pole):
    pozice = int(input("Na jakou pozici chces hrat? "))
    symbol="x"
    active = False
    if pole[pozice] == "x" or pole[pozice] == "o":
        active = True
    while active:
        print(pole)
        pozice = int(input("Na teto pozici jiz je zahrano, vyber jinou pozici: "))
        if pole[pozice] == "x" or pole[pozice] == "o":
            active = True
        else:
            active = False
    return tah(pole, pozice, symbol)

def tah_pocitace(pole):
    symbol = "o"

    if "x" in pole:
        pozice_hrace = pole.index("x")
        pozice = pozice_hrace+1
    if "o" in pole:
        pozice_pc = pole.index(symbol)
        if pole[pozice+1] == "-":
            pozice = pozice_pc + 1
        elif pole[pozice+2] == "-":
            pozice = pozice_pc + 2
        elif pole[pozice-1] == "-":
            pozice = pozice_pc - 1
        elif pole[pozice-2] == "-":
            pozice = pozice_pc - 2
    else:
        pozice = randrange(0,len(pole))
    return tah(pole, pozice, symbol)

def piskvorky1D(x):
    pole=x*"-"
    stav=vyhodnot(pole)
    print(pole)
    while stav:
        pole = tah_hrace(pole)
        print(pole)
        stav = vyhodnot(pole)
        pole = tah_pocitace(pole)
        print(pole)
        stav = vyhodnot(pole)

piskvorky1D(20)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
