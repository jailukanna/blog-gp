---
title: socket example serve (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'serve'


## python serve

Python socket example: serve

```python
class Server(object):
    def __init__(self, hostname, port):
        self.logger = logging.getLogger("server")
        init_logger(self.logger)
        self.hostname = hostname
        self.port = port

    def start(self):
        self.logger.debug("listening")
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.bind((self.hostname, self.port))
        self.socket.listen(1)

        while True:
            conn, address = self.socket.accept()
            self.logger.debug("Got connection")
            process = multiprocessing.Process(target=handle, args=(conn, address))
            process.daemon = True
            process.start()
            self.logger.debug("Started process %r", process)

if __name__ == "__main__":
    logger = logging.getLogger("main")
    init_logger(logger)
    import sys
    port = int(sys.argv[1])
    server = Server("0.0.0.0", port)
    try:
        logger.info("Listening")
        server.start()
    except:
        logger.exception("Unexpected exception")
    finally:
        logger.info("Shutting down")
        for process in multiprocessing.active_children():
            logger.info("Shutting down process %r", process)
            process.terminate()
            process.join()
    logger.info("All done")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
