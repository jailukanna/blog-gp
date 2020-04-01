---
title: socket example service (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'service'


Modules used in program: 
* `import json`

## python service

Python socket example: service

```python
from twisted.internet import protocol, reactor
from twisted.protocols.basic import LineReceiver
import json

class commander(LineReceiver):

    def connectionMade(self):
        self.sendLine("Connected")

    def lineReceived(self, line):
        try:
            jstring = json.loads(line.decode('utf-8'))
            if 'return' in jstring:
                self.sendLine(jstring['return'].encode('utf-8'))
            else:
                raise BaseException(message="Wrong Format")
        except BaseException as ex:
            self.sendLine(ex.message)
            self.connectionLost()


class Factory(protocol.Factory):

    def buildProtocol(self, addr):
        return commander()


#run
reactor.listenTCP(8888, Factory())
reactor.run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
