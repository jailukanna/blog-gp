---
title: PIL example screenshot dmm (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screenshot dmm'


Modules used in program: 
* `import struct`
* `import socket`
* `import time`
* `import sys`
* `import io`

## python screenshot dmm

Python PIL example: screenshot dmm

```python
import io
import sys
import time
import socket
import struct

from PIL import Image

if len(sys.argv) != 2:
    print("Usage: screenshot_dmm.py <filename>")
    sys.exit(1)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect(("dmm.bench", 5025))
# Set data to PNG
s.send(b"HCOP:SDUM:DATA:FORM PNG\r\n")
# Now request a screenshot
s.send(b"HCOP:SDUM:DATA?\r\n")
# Read the first 2 bytes, the definite-length header
# Expected format is `#NMMMMXXXXXXXXXXXXX` where
# `#` indicates definite-length, N is the number of digits for M,
# and MMMM is the number of data bytes, XXXXX are the data bytes.
header = s.recv(2)
assert header[0] == ord("#"), "Expected SCPI definite-length block header"
file_len_len = int(chr(header[1]))
file_len = int(s.recv(file_len_len))
print("File size is", file_len, "bytes")
# We'll store the read data in this bytearray
data = bytearray(file_len)
total_received = 0
# Loop reading data from the socket
while total_received < file_len:
    packet = s.recv(file_len)
    data[total_received:len(packet)] = packet
    total_received += len(packet)
    print("Received", total_received, "bytes")
    time.sleep(0.2)

# Use Pillow to write the file out in whatever format the user wants
img = Image.open(io.BytesIO(data))
img.save(sys.argv[1])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
