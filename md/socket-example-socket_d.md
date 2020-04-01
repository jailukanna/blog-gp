---
title: socket example socket d (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket d'


Modules used in program: 
* `import socket`

## python socket d

Python socket example: socket d

```python
import socket

s = socket.socket()
s.connect(("192.168.1.100", 9999))

while True:
	mensaje = raw_input(">")
	s.send(mensaje)
	if mensaje == "quit":
		break

print("Adios")
s.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
