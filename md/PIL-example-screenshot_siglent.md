---
title: PIL example screenshot siglent (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screenshot siglent'


Modules used in program: 
* `import struct`
* `import socket`
* `import time`
* `import sys`
* `import io`

## python screenshot siglent

Python PIL example: screenshot siglent

```python
import io
import sys
import time
import socket
import struct

from PIL import Image

if len(sys.argv) != 3:
    print("Usage: screenshot.py <IP address> <filename>")
    sys.exit(1)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Normally we'd connect on 5025 for automated use, avoiding the user shell
# prompts. However the Siglent SDG2042X does not listen on 5025 with current
# firmware, only 5024, the interactive shell, so use that instead.
s.connect((sys.argv[1], 5024))
# We need to eat up all the junk shell prompts etc
s.recv(1000)
# Now request a screenshot
s.send(b"SCDP\r\n")
# Read the first 6 bytes, the BMP header
header = s.recv(6)
# Check it's a BMP header
assert header[0:2] == b"BM", "Expected BMP header"
# Read the specified file length, which is the number of bytes we have to read
file_len = struct.unpack("<I", header[2:])[0]
print("File size is", file_len, "bytes")
# We'll store the read data in this bytearray
data = bytearray(file_len)
data[0:6] = header
total_received = 6
# Loop reading data from the socket
while total_received < file_len:
    packet = s.recv(file_len)
    data[total_received:len(packet)] = packet
    total_received += len(packet)
    print("Received", total_received, "bytes")
    time.sleep(0.2)

# Use Pillow to write the file out in whatever format the user wants
img = Image.open(io.BytesIO(data))
img.save(sys.argv[2])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
