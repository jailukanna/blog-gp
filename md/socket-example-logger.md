---
title: socket example logger (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'logger'

Functions in program: 
* `def main():`
* `def init_logger():`

Modules used in program: 
* `import sys`
* `import logging`
* `import socket`
* `import os`

## python logger

Python socket example: logger

```python
import os
import socket
from time import sleep
import logging
from logging import StreamHandler, FileHandler
import sys


def init_logger():
  logger = logging.getLogger("lida")
	logger.setLevel(logging.INFO)
	formatter = logging.Formatter(
		'[%(name)s] %(levelname)s %(asctime)s: %(message)s')
	std_out_handler = StreamHandler(sys.stdout)
	std_out_handler.setFormatter(formatter)
	logger.addHandler(std_out_handler)
	file_handler = FileHandler("log.txt")
	file_handler.setFormatter(formatter)
	logger.addHandler(file_handler)
	return logger	

                                            
def main():
	to_logger, from_parent = socket.socketpair()
	
	pid = os.fork()
	if pid == 0:
		# child's process
		logger = init_logger()
		logger.warning("start logging")
		while True:
			msg = from_parent.recv(100)
			logger.info(msg)
	else:
		# parent's process 
		to_logger.send("hello child process")
		sleep(1)
		to_logger.send("foo bar")
		sleep(1)
		to_logger.send("CRASH!")
		sleep(1)
		
   
main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
