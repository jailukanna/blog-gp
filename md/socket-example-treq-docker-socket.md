---
title: socket example treq-docker-socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'treq-docker-socket'

Functions in program: 
* `def print_stuff(data):`
* `def on_response(response):`
* `def done(reactor):`

## python treq-docker-socket

Python socket example: treq-docker-socket

```python
from treq import get
from twisted.internet.task import react
from twisted.internet.defer import inlineCallbacks
from twisted.internet.endpoints import UNIXClientEndpoint
from twisted.web.iweb import IAgentEndpointFactory
from twisted.web.client import Agent
from zope.interface import implementer


@implementer(IAgentEndpointFactory)
class DockerEndpointFactory(object):
    """
    Connect to Docker's Unix socket.
    """
    def __init__(self, reactor):
        self.reactor = reactor

    def endpointForURI(self, uri):
        return UNIXClientEndpoint(self.reactor, b"/run/docker.sock")


@inlineCallbacks
def done(reactor):
    agent = Agent.usingEndpointFactory(reactor, DockerEndpointFactory(reactor))
    yield get("unix://localhost/v1.30/images/json", agent=agent).addCallback(on_response)


def on_response(response):
    response.json().addCallback(print_stuff)


def print_stuff(data):
    print(data)


if __name__ == '__main__':
    react(done)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
