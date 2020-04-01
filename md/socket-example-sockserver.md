---
title: socket example sockserver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockserver'

Functions in program: 
* `def recv_LED_data(conn):`
* `def LED_off():`

Modules used in program: 
* `import time`
* `import socket`
* `import pigpio`

## python sockserver

Python socket example: sockserver

```python
#!/usr/bin/env python3

import pigpio
import socket
import time

NETHOST = ''
PORT = 2020
WAITTIME = 10

pi = pigpio.pi()

LED1R = 10
LED1G = 9
LED1B = 11
LED2R = 17
LED2G = 27
LED2B = 22

def LED_off():
   pi.set_PWM_dutycycle(LED1R, 0)
   pi.set_PWM_dutycycle(LED1G, 0)
   pi.set_PWM_dutycycle(LED1B, 0)
   pi.set_PWM_dutycycle(LED2R, 0)
   pi.set_PWM_dutycycle(LED2G, 0)
   pi.set_PWM_dutycycle(LED2B, 0)

def recv_LED_data(conn):
   str = '1'
   while (len(str) > 0):
      try:
         str = conn.recv(1024)
         if len(str) == 0:
            break
         str = str[:-2]
         str = str.decode('utf-8')
         innerR, innerG, innerB, outerR, outerG, outerB = str.split(" ")
         pi.set_PWM_dutycycle(LED1R, innerR)
         pi.set_PWM_dutycycle(LED1G, innerG)
         pi.set_PWM_dutycycle(LED1B, innerB)
         pi.set_PWM_dutycycle(LED2R, outerR)
         pi.set_PWM_dutycycle(LED2G, outerG)
         pi.set_PWM_dutycycle(LED2B, outerB)
      except ValueError:
         pass
   print("Client Disconnected.")

while True:
   try:
      with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
         s.bind((NETHOST, PORT))
         s.listen(2)
         conn, addr = s.accept()
         with conn:
            print('Connected from ', addr)
            recv_LED_data(conn)
         s.close()
   except BrokenPipeError as error:
      print(error)
      print("Dropped connection. Attempting to reconnect...")
      LED_off()
      time.sleep(WAITTIME)
   except OSError as error:
      print(error)
      print("Connection failed. Retrying...")
      LED_off()
      time.sleep(WAITTIME)

LED_off()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
