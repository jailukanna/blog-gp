---
title: socket example Sender (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'Sender'


Modules used in program: 
* `import md5`
* `import sys`
* `import os`
* `import json`
* `import socket`

## python Sender

Python socket example: Sender

```python
#! /usr/bin/python

import socket
import json
import os
import sys
import md5

ACK_MESSAGE="HEADER_RECEIVED"
CONTENT_ACK_MESSAGE="CONTENT_RECEIVED"

folderPath = sys.argv[1]

for dirname, dirnames, filenames in os.walk(folderPath):
    for filename in filenames:
        filePath = (dirname + '/' + filename).replace("\\", "/")
        print("FOUND FILE: ", filePath)
        length = os.stat(filePath).st_size
        
        f = open(filePath, 'rb')
        fileContent = f.read()
        f.close()

        m = md5.new()
        m.update(fileContent)

        header = {}
        header['name'] = filename
        header['length'] = str(length)
        header['folder'] = dirname.replace("\\", "/").replace(folderPath, '')
        header['md5'] = m.hexdigest()
        
        print(header)

        done = False
        while not done:
            try:
                eggs = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                eggs.connect(('localhost', 8080))
                eggs.sendall(json.JSONEncoder().encode(header))
                response = eggs.recv(len(ACK_MESSAGE))
                print(response)
                eggs.sendall(fileContent)
                response = eggs.recv(len(CONTENT_ACK_MESSAGE))
                print(response)
                eggs.close()
                done = True
            except:
                print('Retrying...')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
