---
title: turtle example wombo-combo piskvorky 1d (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo piskvorky 1d'

Functions in program: 
* `def piskvorky1D():`
* `def tah_hrace(pole):`
* `def vyhodnot(pole):`

## python wombo-combo piskvorky 1d

Python turtle example: wombo-combo piskvorky 1d

```python
# retezec = "Caute, tady Zuza \n a zdravi vas!"
# print(retezec)
#
# print("cago\n!!")
#
# print(r'C:\Users\zpiskoro\Desktop\pyladies_vol_2\ukoly')
#
#
# retezec = 'cokolada'
# print(retezec[2:6])
#
# print(retezec.upper())


# def zamen(retezec, pozice, znak):
#     novy_retezec = retezec[:pozice] + znak + retezec[pozice+1:]
#     return novy_retezec


from GitHubProjects import ai


def vyhodnot(pole):
    if 'xxx' in pole:
        print('Vyhral x')
        return False
    elif 'ooo' in pole:
        print('Vyhral o')
        return False
    else:
        return True

def tah_hrace(pole):


    try:
        pozice = int(input('Na jakou pozici chces hrat?: '))
    except ValueError:
        pozice = random.randrange(len(pole))
        print("Toto neni validni cislo...")
    symbol = 'x'
    return ai.tah(pole, pozice, symbol)

def piskvorky1D():
    pole = 20*'-'
    stav = vyhodnot(pole)
    while stav:
        pole = tah_hrace(pole)
        print(pole)
        stav = vyhodnot(pole)
        if stav == True:
            pole = ai.tah_ai(pole)
            print(pole)
            stav = vyhodnot(pole)

piskvorky1D()














```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
