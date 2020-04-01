---
title: socket example serv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'serv'

Functions in program: 
* `def main():`
* `def Bind_UDP():`
* `def Bind_TCP():`

Modules used in program: 
* `import sys, socket, threading, readline`

## python serv

Python socket example: serv

```python
#!/usr/bin/env python3

import sys, socket, threading, readline

IP = sys.argv[1]
PORT = int(sys.argv[2])

UDP = socket.SOCK_DGRAM
TCP = socket.SOCK_STREAM

# TCP
# Bind and listen to incoming data from a single source
def Bind_TCP():
	global sock2
	sock2 = socket.socket(socket.AF_INET, TCP)
	sock2.bind(('0.0.0.0', PORT))
	sock2.listen(1)
	conn, addr = sock2.accept()
	while 1:
		data = conn.recv(128)
		if not data: continue
		print(f"Received: {data.decode('utf-8')}")

# UDP
# Bind and listen to any incoming data
def Bind_UDP():
	global sock2
	sock2 = socket.socket(socket.AF_INET, UDP)
	sock2.bind(('0.0.0.0', PORT))
	while 1:
		data, addr = sock2.recvfrom(128)
		print(f"Received: {data.decode('utf-8')}")

def main():
	global sock1
	listen = threading.Thread(target=Bind_UDP)  # Bind in a separate thread
	listen.setDaemon(True)                      # Exit when script ends
	listen.start()
	print(f"Target: {IP}:{PORT}")
	sock1 = socket.socket(socket.AF_INET, UDP)
	while 1:
		sock1.sendto(input().encode("utf-8"), (IP, PORT))

try:
	main()
except KeyboardInterrupt:
	sock1.close()
	sock2.close()
	exit("")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
