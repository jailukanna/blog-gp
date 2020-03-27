---
title: turtle example wombo-combo Assigments (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo Assigments'


## python wombo-combo Assigments

Python turtle example: wombo-combo Assigments

```python
# Error testing

# print("Ahoj"-0)               TypeError: unsupported operand type(s) for -: str and int
# print(6/0)                    ZeroDivisionError: division by zero
# print(x)                      NameError: name 'x' is not defined
# a = input("Zmackni ctrl+c")     Selects the input text
#

#    print("nic")                IndentationError: unexpected indent

#
# if x<0:                         IndentationError: unindent does not match any outer indentation level
#    print("niic 2")
#  print("neco?")
# else:
#     print("as")
#
# print("Ahoj"                    SyntaxError: unexpected EOF while parsing
#
# xy = x ! 5                      SyntaxError: invalid syntax
#
# print(1,2,,3)                   SyntaxError: invalid syntax, depends where though
# print("ahoj"/"a")


### Task 6
# a = int(input("Zadej stranu a: "))
# povrch = a**2*6
# objem = a**3
# print("Povrch krychle je {} cm2 a objem {} cm3.".format(povrch,objem))

from random import randrange
jednicky = 0
dvojky = 0
nuly = 0
i = 0

while i<100000:
    cislo = randrange(3)
    if cislo == 0:
        nuly = nuly+1
    elif cislo == 1:
        jednicky = jednicky+1
    else:
        dvojky = dvojky+1
    i=i+1

print("Cislo 0: {}x \nCislo 1: {}x \nCislo 2: {}x".format(nuly,jednicky,dvojky))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
