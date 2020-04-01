---
title: socket example tcp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcp'


Modules used in program: 
* `import msvcrt`
* `import string`
* `import socket`
* `import os`
* `import sys`

## python tcp

Python socket example: tcp

```python
#!/usr/bin/env python
import sys
import os
import socket
import string
# windows only
import msvcrt

if len(sys.argv) != 3:
        print("usage: %s target_ip target_port" %(os.path.split(sys.argv[0])[1]))
        sys.exit(0)

try:
        target_ip = socket.gethostbyname(sys.argv[1])
except:
        print("unknown target_ip: %s" %(sys.argv[1]))
        sys.exit(0)
try:
        target_port = int(sys.argv[2])
except:
        print("target_port: %s error" %(sys.argv[2]))
        sys.exit(0)
if target_port <= 0:
        print("target_port: %s error" %(sys.argv[2]))
        sys.exit(0)

output_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
try:
        output_socket.connect((target_ip,target_port))
except:
        print('connect error')
        sys.exit(0)
output_socket.settimeout(0.5)        # non-blocking
buf = ""
buf_output = ""
flag_timeout=0
while 1:
        # socket
        try:
                buf = output_socket.recv(1024)
        except socket.timeout:
                flag_timeout=1
                pass
        except:
                break
        if flag_timeout != 1:
                if len(buf) != 0:
                        buf_temp = ' '.join(map(lambda c: "%02X" %ord(c), buf))
                        print(buf_temp)
                else:
                        print('connection closed')
                        break
        else:
                flag_timeout=0
        if msvcrt.kbhit():        # if a keypress is waiting to be read
                buf = sys.stdin.readline()
                if buf == '\x0A':
                        output_socket.send(buf_output)
                        continue
                try:
                        buf_output = ""
                        buf_temp = buf.split()
                        for c in buf_temp:
                                buf_output += chr(string.atoi('0x'+c, 16))
                        output_socket.send(buf_output)
                except:
                        print('input format error')
output_socket.close()
sys.exit(0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
