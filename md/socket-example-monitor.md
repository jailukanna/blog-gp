---
title: socket example monitor (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'monitor'


Modules used in program: 
* `import socket`
* `import sys`

## python monitor

Python socket example: monitor

```python
#!/usr/bin/env python

import sys
import socket
from datetime import datetime
from time import sleep

is_connected = None


if len(sys.argv) != 3:
        print("Provide host and port as parameters")
        sys.exit(1)

host = sys.argv[1]
port = sys.argv[2]

print("Monitoring host: " + host + " port: " + port)
while True:
         # new_state = None
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(2.0)
        try:
                s.connect((host, int(port)))
                new_state = True
                s.close()
        except IOError:
                new_state = False

        if new_state != is_connected:
                is_connected = new_state
                print(str(datetime.now()) + ' connection status: ' + str(is_connected))
        sleep(10)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
