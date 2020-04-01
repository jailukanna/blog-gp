---
title: socket example autotest (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'autotest'


Modules used in program: 
* `import time`
* `import sys`
* `import subprocess`

## python autotest

Python socket example: autotest

```python
"""
Code in other all other files adapted from here:
https://stackoverflow.com/q/58424446

This is just a simple script that runs processes for the server and client.

My comments in server.py and client.py all start with '##'.

shared.py sets the global variables shared by the client and the server,
and it modifies a handful of functions they use to improve logging.

The arguments to this script are the port for the server to use
and the number of clients to test.

If the output includes the phrase "client thread completed" a number of
times equal to the number of clients, the behavior was what I believe the
asker expected (i.e. the server processed each client). However, the script
will not terminate, and will need to be closed manually.
"""
import subprocess
import sys
import time

try:
    port = int(sys.argv[1]) # just make sure it's an int
    port = str(port)
except IndexError:
    print('usage: test.py PORT [NUMBER_OF_CLIENTS]', file=sys.stderr)
    sys.exit(1)

if len(sys.argv) >= 3:
    clients = int(sys.argv[2])
    if clients > 26:
        print("Error: can only have up to 26 clients")
        sys.exit(1)
else:
    clients = 3

procs = [subprocess.Popen([sys.executable, 'server.py', port, '(server)', str(clients)])]

time.sleep(1) # Give the server a moment to start

procs.extend(subprocess.Popen([sys.executable, 'client.py', port, '(client {})'.format(i)])
             for i in range(clients))

for proc in procs:
    proc.wait() # Note: this will never finish, as the server runs forever


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
