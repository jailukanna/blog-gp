---
title: socket example ws-client-plus-gpio (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ws-client-plus-gpio'

Functions in program: 
* `def destroy():`
* `def setup():`

Modules used in program: 
* `import RPi.GPIO as GPIO`
* `import logging`
* `import asyncio`
* `import tornado.websocket`

## python ws-client-plus-gpio

Python socket example: ws-client-plus-gpio

```python
import tornado.websocket
import asyncio
import logging
import RPi.GPIO as GPIO
from time import sleep

LedPin = 21

def setup():
    GPIO.setwarnings(False)
    GPIO.cleanup()
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(LedPin, GPIO.OUT)
    GPIO.output(LedPin, GPIO.HIGH)


def destroy():
    GPIO.output(LedPin, GPIO.LOW)
    GPIO.cleanup()


async def main():
    logging.warn('Starting main')
    
    conn = await tornado.websocket.websocket_connect('ws://akit.kotoblog.pp.ua/chatsocket')

    conn.write_message('device')

    setup()
    while True:
        GPIO.output(LedPin, GPIO.HIGH)
        await asyncio.sleep(0.3)
        GPIO.output(LedPin, GPIO.LOW)
        await asyncio.sleep(0.3)

        message = await conn.read_message()
        try:
            value = int(message)
            logging.warn(value)
        except:
            logging.warn('"%s" is not integer. Ignore it', message)


if __name__ == "__main__":
    logging.warn('Starting program')

    try:
        ioloop = asyncio.get_event_loop()
        ioloop.run_until_complete(main())
        ioloop.close()
    except KeyboardInterrupt:
        destroy()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
