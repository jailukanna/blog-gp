---
title: turtle example wombo-combo ai (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo ai'

Functions in program: 
* `def tah(pole, pozice, symbol):`
* `def tah_ai(pole):`

Modules used in program: 
* `import random as rnd`

## python wombo-combo ai

Python turtle example: wombo-combo ai

```python
import random as rnd

def tah_ai(pole):

    symbol = "o"
    while True:
        if "x" not in pole:
            pozice = rnd.randrange(2, len(pole)-3)
            return tah(pole, pozice, symbol)

        if "o-o" in pole:
            pozice = pole.index("o-o")+1
            return tah(pole, pozice, symbol)

        if "oo-" in pole:
            pozice = pole.index("oo-")+2
            return tah(pole, pozice, symbol)

        if "-oo" in pole:
            pozice = pole.index("-oo")
            return tah(pole, pozice, symbol)

        if "xx-" in pole:
            pozice = pole.index("xx-")+2
            return tah(pole, pozice, symbol)

        if "-xx" in pole:
            pozice = pole.index("-xx")
            return tah(pole, pozice, symbol)

        if "x-x" in pole:
            pozice = pole.index("x-x")+1
            return tah(pole, pozice, symbol)

        if "-x-" in pole and (pole.index("-x-") != 0 or pole.index("-x-") != 1):
            pozice = pole.index("-x-")
            return tah(pole, pozice, symbol)

        if "--o" in pole:
            pozice = pole.index("--o")+1
            return tah(pole, pozice, symbol)

        if "o--" in pole:
            pozice = pole.index("o--")+1
            return tah(pole, pozice, symbol)

        if pole[1] == "x" and pole[2] == "-":
            pozice = 2
            return tah(pole,pozice,symbol)
        elif pole[-2] == "x" and pole[-3] == "-":
            pozice = -3
            return tah(pole, pozice, symbol)

        elif "---" in pole:
            pozice = pole.index("---")+1
            return tah(pole, pozice, symbol)
        else:
            pozice = rnd.randrange(0,len(pole))
            if "-" not in pole[pozice]:
                print("AI hraje na obsazene pole {}".format(pozice))
            else:
                return tah(pole,pozice,symbol)


def tah(pole, pozice, symbol):
    return pole[:pozice]+symbol+pole[pozice+1:]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
