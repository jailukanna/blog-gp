---
title: socket example srv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'srv'


Modules used in program: 
* `import socket`

## python srv

Python socket example: srv

```python
#Szerver, bejövő kapcsolat esetén vár egy üzenetre,
#majd erre az üzenetre küld egy választ

import socket

srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
srv.bind(("0.0.0.0", 5555))
srv.listen()
client, client_address = srv.accept()

message = client.recv(1024)
print(message)
client.send(b"Hello client!\n")

client.close()
srv.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
