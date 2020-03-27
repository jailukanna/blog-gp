---
title: turtle example abstraer-procedimientos (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'abstraer-procedimientos'

Functions in program: 
* `def espiral(tortuga):`

## python abstraer-procedimientos

Python turtle example: abstraer-procedimientos

```python
from turtle import Turtle

gamera = Turtle()

# Podríamos hacer muchas cosas padres con sólo repetir:
gamera.forward(50)
gamera.left(30)
gamera.forward(50)
gamera.left(30)
gamera.forward(50)
gamera.left(30)
#   ...

# Pero Python nos provee de un mecanismo para hacer esta repetición:
for i in range(100):
  gamera.forward(100)
  gamera.left(60)
  
# Incluso podemos modificar cada iteración:
for i in range(100):
  gamera.forward(100-i)
  gamera.left(60)
  
# Y podemos no sólo abstraer el bloque que queremos repetir,
# sino que también podemos abstraer el procedimiento completo:
def espiral(tortuga):
  for i in range(100):
    tortuga.forward(100-i)
    tortuga.left(60)
    
# Ahora el procedimiento se llama espiral, 
# y podemos hacer que nuestra tortuga lo siga
espiral(gamera)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
