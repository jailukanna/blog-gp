---
title: socket example ssh agent listpubkeys (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ssh agent listpubkeys'

Functions in program: 
* `def main(sockname):`
* `def process(sock):`
* `def recv_int(s, n, byteorder='big'):`

Modules used in program: 
* `import contextlib`
* `import struct`
* `import sys`
* `import socket`
* `import os`

## python ssh agent listpubkeys

Python socket example: ssh agent listpubkeys

```python
import os
import socket
import sys
import struct
import contextlib

# Reference for the data structure / interaction:
# https://tools.ietf.org/html/draft-miller-ssh-agent-00

# Good to read:
# http://ptspts.blogspot.com/2010/06/how-to-use-ssh-agent-programmatically.html

# Section 5.1 & 5.2
SSH2_AGENTC_REQUEST_IDENTITIES = 11
SSH2_AGENT_IDENTITIES_ANSWER = 12


class ConsumableBytes(object):

    def __init__(self, bytes_=None):
        self._bytes = bytes_

    def chomp(self, n):  # type: (int) -> None
        self._bytes = self._bytes[n:]

    def get_int(self, n, endian='big'):  # type: (int, str) -> int
        rslt = int.from_bytes(self._bytes[:n], byteorder=endian)
        self.chomp(n)
        return rslt

    def get_bytes(self, n):  # type: (int) -> bytes
        rslt = self._bytes[:n]
        self.chomp(n)
        return rslt

    def __repr__(self):
        return str(self._bytes)

    @property
    def is_empty(self):  # type: () -> bool
        return len(self._bytes) == 0


def recv_int(s, n, byteorder='big'):
    bts = s.recv(n)
    return int.from_bytes(bts, byteorder=byteorder)


def process(sock):
    command = bytes([SSH2_AGENTC_REQUEST_IDENTITIES])
    data = struct.pack(">Lc", len(command), command)

    sock.sendall(data)

    mlen = int.from_bytes(sock.recv(4), byteorder='big')
    print('Msglen', mlen)

    msg = ConsumableBytes(sock.recv(mlen))

    resp = msg.get_int(1)
    print('Respcode', resp)
    if resp != SSH2_AGENT_IDENTITIES_ANSWER:
        raise RuntimeError()

    nkeys = msg.get_int(4)
    print('Nkeys', nkeys)

    for i in range(0, nkeys):
        print('===== Key #{} ====='.format(i+1))

        # Overall length of the pubkey structure
        klen = msg.get_int(4)
        print('Keylen', klen)

        tlen = msg.get_int(4)
        print('Tlen', tlen)

        typ_ = msg.get_bytes(tlen)
        print('Typ_', typ_.decode())

        n_len = msg.get_int(4)
        print('n_len', n_len)

        n_val = msg.get_int(n_len)
        print('n_val', n_val)

        e_len = msg.get_int(4)
        print('e_len', e_len)

        e_val = msg.get_int(e_len)
        print('e_val', e_val)

        # pubkeys has no "d", "iqmp", "p", and "q" components

        cmt_len = msg.get_int(4)
        print('cmt_len', cmt_len)

        cmt = msg.get_bytes(cmt_len)
        print('cmt', cmt.decode())

    if not msg.is_empty:
        print('Leftover:', repr(msg))


def main(sockname):
    with contextlib.closing(
            socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)) as sock:
        try:
            sock.connect(sockname)
            process(sock)
        except socket.error as msg:
            print(msg, file=sys.stderr)
            sys.exit(1)
        except Exception as e:
            raise


if __name__ == '__main__':
    main(os.environ['SSH_AUTH_SOCK'])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
