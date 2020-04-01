---
title: socket example recv fds (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'recv fds'

Functions in program: 
* `def recv_fds(sock, nfds):`
* `def retry_on_eintr(f):`
* `def align(l):`

Modules used in program: 
* `import socket`
* `import os`
* `import errno`

## python recv fds

Python socket example: recv fds

```python
"""
recv_fds reads file descriptors from a unix domain socket. This might be useful
if you were, say, wanting to implement short-circuit reads in an HDFS client.
The code is broadly based on DomainSocketImpl::receiveFileDescriptors in libhdfs3
and Java_org_apache_hadoop_net_unix_DomainSocket_receiveFileDescriptors0
in libhadoop.

The tricky thing here is that we need to call recvmsg, which isn't exposed in
the socket module for python 2.7. However, we can access it via ctypes. I've
tried the code here out on my machine (x86_64, Ubuntu 14.14, Python 2.7.6) and
it appears to work.
"""

from ctypes import addressof, cast, c_int, c_size_t, c_void_p,\
    create_string_buffer, CDLL, get_errno, pointer, POINTER, sizeof, Structure
from ctypes.util import find_library
import errno
import os
import socket

libc = CDLL(find_library("c"), use_errno=True)

class IOVec(Structure):
    _fields_ = [('iov_base', c_void_p),
                ('iov_len', c_size_t)]

class MsgHdr(Structure):
    _fields_ = [('msg_name', c_void_p),
                ('msg_namelen', c_int),
                ('msg_iov', POINTER(IOVec)),
                ('msg_iovlen', c_size_t),
                ('msg_control', c_void_p),
                ('msg_controllen', c_size_t),
                ('msg_flags', c_int)]

class CMsgHdr(Structure):
    _fields_ = [('cmsg_len', c_size_t),
                ('cmsg_level', c_int),
                ('cmsg_type', c_int)]

SOL_SOCKET = socket.SOL_SOCKET
SCM_RIGHTS = 1

def align(l):
    wordsize = sizeof(c_size_t)
    n_words = (l + wordsize -1) // wordsize
    return n_words * wordsize

def retry_on_eintr(f):
    rc = f()
    while rc < 0 and get_errno() == errno.EINTR:
        rc = f()
    return rc

def recv_fds(sock, nfds):
    iov = pointer(IOVec())
    iov[0].iov_base = cast(create_string_buffer(1), c_void_p)
    iov[0].iov_len = 1

    fd_size = sizeof(c_int) * nfds
    aux_size = align(sizeof(CMsgHdr)) + align(fd_size)
    aux = create_string_buffer(aux_size)

    msg = MsgHdr()
    msg.msg_iov = iov
    msg.msg_iovlen = 1;
    msg.msg_control = cast(aux, c_void_p)
    msg.msg_controllen = aux_size
    cmsg = cast(aux, POINTER(CMsgHdr))
    cmsg[0].cmsg_level = SOL_SOCKET
    cmsg[0].cmsg_type = SCM_RIGHTS
    cmsg[0].cmsg_len = align(sizeof(CMsgHdr)) + fd_size
    msg.msg_controllen = cmsg[0].cmsg_len
    msgptr = pointer(msg)

    sock_fd = sock.fileno()
    rc = retry_on_eintr(lambda: libc.recvmsg(sock_fd, msgptr, 0))

    if rc < 0:
        err = get_errno()
        raise OSError(err, os.strerror(err))
    elif rc == 0:
        n_recvd_fds = 0
    else:
        n_recvd_fds = (cmsg[0].cmsg_len - sizeof(CMsgHdr)) / sizeof(c_int)

    fdptr = cast(addressof(cmsg[1]), POINTER(c_int))
    return [fdptr[i] for i in range(n_recvd_fds)]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
