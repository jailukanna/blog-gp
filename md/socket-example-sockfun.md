---
title: socket example sockfun (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockfun'

Functions in program: 
* `def rpc(*args):`
* `def app(env, sr):`

Modules used in program: 
* `import uwsgi`
* `import time`
* `import struct`
* `import sys`
* `import socket`
* `import os`

## python sockfun

Python socket example: sockfun

```python
# encoding: utf8
"""
save this as sockfun.py, then::

    git clone https://github.com/unbit/uwsgi.git && cd uwsgi
    python2 setup.cpyext.py build
    PYTHONPATH=build/lib.linux-x86_64-2.7 python2 sockfun.py
"""

from __future__ import print_function

import os
import socket
import sys
import struct
import time
import uwsgi


class Blockvars(object):

    def __init__(self, array):
        if hasattr(array, 'keys'):
            array = [str(i) for kv in array.items() for i in kv]
        self.array = array or tuple()

    def __len__(self):
        return (2 * len(self.array)) + sum(map(len, self.array))

    def __repr__(self):
        return '<{0.__class__.__name__}: {0.array}>'.format(self)

    def encode(self):
        return ''.join(self.iter_encode())

    def iter_encode(self):
        for item in self.array:
            yield struct.pack('<H', len(item))
            yield item


class UwsgiRequest(object):

    def __init__(self, modifier1=0, modifier2=0, blockvars=None, body=None):
        # LANG: 0=python; 5=perl; lua=6; ruby=7; jvm=8; go=11; php=14; mono=15
        # UTIL: 17=spooler; 110=signal; 111=cache; 173=rpc
        self.modifier1 = chr(modifier1)
        self.modifier2 = chr(modifier2)
        self.blockvars = Blockvars(blockvars)
        self.body = body or tuple()
        if isinstance(body, basestring):
            self.body = (body,)

    def __repr__(self):
        return '<{0.__class__.__name__}: {1} {2}>'.format(
                self, ord(self.modifier1), ord(self.modifier2),
                )

    def encode(self):
        return ''.join(self.iter_encode())

    def iter_encode(self):
        # uwsgi header::
        #     struct uwsgi_packet_header {
        #         uint8_t modifier1;
        #         uint16_t datasize;
        #         uint8_t modifier2;
        #     };
        yield self.modifier1
        yield struct.pack('<H', len(self.blockvars))
        yield self.modifier2

        # blockvars::
        #    struct uwsgi_var {
        #        uint16_t key_size;
        #        uint8_t key[key_size];
        #        uint16_t val_size;
        #        uint8_t val[val_size];
        #    }
        for blockvar in self.blockvars.iter_encode():
            yield blockvar

        # body requires CONTENT_LENGTH above for use as a simple request, else
        # must implement HTTP/1.1 for uWSGI to read it on other end
        for chunk in self.body:
            yield chunk


def app(env, sr):
    return '(server) app: {!r} + {!r}'.format(
            {k:v for k,v in env.items() if 'wsgi.' not in k},
            env['wsgi.input'].read(),
            )


def rpc(*args):
    return '(server) rpc: {!r}'.format(args)


server = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
server.bind('')
server.listen(1)
address = server.getsockname()

pid = os.fork()
if pid:
    # server/parent
    # uWSGI must be built with setup.cpyext.py for this to work
    uwsgi.setup(
            '--puwsgi-socket=fd://{}'.format(server.fileno()),
            '--module=__main__:app',
            '--enable-threads',
            # server and client share stdout/err, this removes jitter
            '--disable-logging',
            )
    uwsgi.register_rpc('rpc', rpc)
    uwsgi.run()
else:
    # client/child
    server.close()
    while True:
        for i, request in (
                (1, UwsgiRequest(0, 0, {'key': 'val'}, '')),
                (3, UwsgiRequest(0, 0, {'key': 'val22'}, '')),
                ):
            client = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
            client.connect(address)
            for i in xrange(i):
                client.sendall(request.encode())
            response = client.recv(8192)
            if response[0] == chr(173):
                # remove rpc/uwsgi header
                response = response[4:]
            print('client: ' + response)
            time.sleep(0.5)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
