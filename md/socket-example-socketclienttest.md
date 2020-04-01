---
title: socket example socketclienttest (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketclienttest'


Modules used in program: 
* `import sys`
* `import logging`
* `import time`

## python socketclienttest

Python socket example: socketclienttest

```python
from socket import *
import time
import logging
from logging.handlers import TimedRotatingFileHandler
import sys

if len(sys.argv) is 1:
    print("need port number")
    exit(0)

ipaddr = '127.0.0.1'
port = int(sys.argv[1])
#port = 8801

loglevel = logging.INFO
logfile = 'cl_log_%s_%d.txt' % (ipaddr, port)

print(logfile)

root_logger = logging.getLogger('')

root_logger.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s.%(msecs)03d %(levelname)s: %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S')

# stdout logging
sh = logging.StreamHandler()
sh.setFormatter(formatter)
root_logger.addHandler(sh)

# file log & rotate
fh = TimedRotatingFileHandler(logfile,
                               when="midnight",
                               backupCount=5)
fh.suffix = "%Y-%m-%d" # or anything else that strftime will allow
fh.setFormatter(formatter)
root_logger.addHandler(fh)

while True:
    logging.info("try connecting %s..." % (port))
    tcpSock = socket(AF_INET, SOCK_STREAM)
    try:
        tcpSock.connect((ipaddr, port))
        logging.info("connected...")
        while True:
            data = tcpSock.recv(4096*1000000)
            logging.info(data.decode('cp949')[:100])
    except Exception as ex:
        logging.error(ex)

    time.sleep(2)

tcpSock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
