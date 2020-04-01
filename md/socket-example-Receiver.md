---
title: socket example Receiver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'Receiver'


Modules used in program: 
* `import os`
* `import sys`
* `import md5`
* `import socket`
* `import json`

## python Receiver

Python socket example: Receiver

```python
#! /usr/bin/python

import json
import socket
import md5
import sys
import os

OUTPUT_PATH = '/mnt/disk1/files/shared_folder/Fernando/new_images/'

eggs = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
eggs.bind(('', 8080))
eggs.listen(1)

while True:
    channel, details = eggs.accept()
    print('Connection received')
    contentHeader = channel.recv(2048)
    if (len(contentHeader) > 0):
        print(contentHeader)
        try:
            header = json.JSONDecoder().decode(contentHeader)
            print(header)
            channel.send("HEADER_RECEIVED")
            folderPath = OUTPUT_PATH+'/'+header['folder']
            try:
                print('Creating path:', folderPath)
                os.makedirs(folderPath)
            except Exception as e:
                print(e)
                pass
            expectedContentSize = int(header['length'])
            outputFile = folderPath+'/'+header['name']
            if not os.path.isfile(outputFile):
                print('Waiting for content...')
                fileContent = ''
                while True:
                    if (len(fileContent) == expectedContentSize):
                        print("File complete")
                        break
                    sys.stdout.write('%s/%s\r' % (len(fileContent), expectedContentSize))
                    sys.stdout.flush()
                    data = channel.recv(4096)
                    if not data:
                        break
                    fileContent += data

                m = md5.new()
                m.update(fileContent)
                print("MD5 Received:", m.hexdigest())

                #print(fileContent)
                f = open(outputFile, 'wb')
                f.write(fileContent)
                f.close()
            else:
                data = channel.recv(expectedContentSize)
                print('File is already present')
            channel.send("CONTENT_RECEIVED")
        except Exception as e:
            print(e)
    channel.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
