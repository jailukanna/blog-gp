---
title: socket example precise client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'precise client'


Modules used in program: 
* `import socket`
* `import pyaudio`

## python precise client

Python socket example: precise client

```python
#!/usr/bin/env python3

import pyaudio
import socket

ADDRESS = ('localhost', 10000)
CHUNK_SIZE = 2048

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(ADDRESS)

audio = pyaudio.PyAudio()
stream = audio.open(
    format=pyaudio.paInt16, channels=1,
    rate=16000, input=True, frames_per_buffer=CHUNK_SIZE
)

try:
    while True:
        sock.sendall(stream.read(CHUNK_SIZE))
finally:
    stream.stop_stream()
    stream.close()
    audio.terminate()
    sock.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
