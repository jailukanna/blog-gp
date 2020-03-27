---
title: turtle example excepciones (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'excepciones'

Functions in program: 
* `def derecha(pasos):`
* `def izquierda(pasos):`

## python excepciones

Python turtle example: excepciones

```python
from turtle import Turtle

squirtle = Turtle()

def izquierda(pasos):
     squirtle.left(90)
     squirtle.forward(pasos)

def derecha(pasos):
     squirtle.right(90)
     squirtle.forward(pasos)
      
pasos_a_dar = 100
acciones = {
  "izquierda": izquierda,
  "derecha": derecha
}

# loop
while True:
  respuesta = input("Dame un número o una acción.")
  
  try:
    # Es un número
    pasos_a_dar = int(respuesta) 
  except ValueError:
    # Nope, era una acción
    acción = acciones[respuesta]
    acción(pasos_a_dar)
   

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
