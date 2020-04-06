---
title: mysql example th pinger 2db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'th pinger 2db'

Functions in program: 
* `def thread_pinger(i, q):`

Modules used in program: 
* `import re`
* `import time`
* `import subprocess`
* `import mysql.connector`

## python th pinger 2db

Python mysql example: th pinger 2db

```python
#!/usr/bin/env python
# ping a list of host with threads for increase speed
# design to use data from/to SQL database
# use standard linux /bin/ping utility

from threading import Thread
import mysql.connector
import subprocess
try:
    import queue
except ImportError:
    import Queue as queue
import time
import re

# some global vars
num_threads = 30
ips_q = queue.Queue()
out_q = queue.Queue()

# thread code : wraps system ping command
def thread_pinger(i, q):
    """Pings hosts in queue"""
    while True:
        # get an IP item form queue
        item = q.get()
        # ping it
        args=['/bin/ping', '-c', '1', '-W', str(item['timeout']),
              str(item['ip'])]
        p_ping = subprocess.Popen(args,
                                  shell=False,
                                  stdout=subprocess.PIPE)
        # save ping stdout
        p_ping_out = str(p_ping.communicate()[0])
        # ping return 0 if up
        if (p_ping.wait() == 0):
          # rtt min/avg/max/mdev = 22.293/22.293/22.293/0.000 ms
          search = re.search(r'rtt min/avg/max/mdev = (.*)/(.*)/(.*)/(.*) ms',
                             p_ping_out, re.M|re.I)
          item['up']  = True
          item['rtt'] = search.group(2)
        else:
          item['up']  = False
        # update output queue
        out_q.put(item)
        # update queue : this ip is processed 
        q.task_done()

# start the thread pool
for i in range(num_threads):
    worker = Thread(target=thread_pinger, args=(i, ips_q))
    worker.setDaemon(True)
    worker.start()

# build IP array
ips = []
for i in range(1,200):
    ips.append("192.168.0."+str(i))

# main loop
while True:
    # retreive data from DB
# add SQL here
    # test start time
    start = time.time()
    # fill queue
    for ip in ips:
        ips_q.put({'ip': ip, 'timeout': 1})
    # wait until worker threads are done to exit    
    ips_q.join()
    # display result
    print("next:")
    while True:
        try:
            msg = out_q.get_nowait()
        except queue.Empty:
            break
        if msg['up']:
            print(msg)
    # test start end
    end = time.time()
    loop_time = round(end - start, 2)
    print("loop time: %s" % (loop_time))
    # update DB
#add SQL here
    # wait 5s before next cycle
    time.sleep(5.0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
