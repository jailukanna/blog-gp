---
title: tkinter example mem freeze monitor (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'mem freeze monitor'

Functions in program: 
* `def main():`

Modules used in program: 
* `import tkinter`
* `import tkinter as tk`
* `import psutil, time`

## python mem freeze monitor

Python tkinter example: mem freeze monitor

```python
#!/usr/bin/env python3

# Memory monitor script. Starts killing highest-mem-usage processes
# (e.g.  usually chrome tabs) when RAM exceeds some threshold (e.g. 92%).
# When running without swap, this helps to avoid freezes.
#
# Source: https://askubuntu.com/a/1018733/159633
#
# Run with sudo

import psutil, time
import tkinter as tk
from subprocess import Popen, PIPE
import tkinter
from tkinter import messagebox
root = tkinter.Tk()
root.withdraw()

RAM_USAGE_THRESHOLD = 92
MAX_NUM_PROCESS_KILL = 100

def main():
    if psutil.virtual_memory().percent >= RAM_USAGE_THRESHOLD:
        # Clear RAM cache
        mem_warn = "Memory usage critical: {}%\nClearing RAM Cache".\
            format(psutil.virtual_memory().percent)
        print(mem_warn)
        Popen("notify-send \"{}\"".format(mem_warn), shell=True)
        print("Clearing RAM Cache")
        print(Popen('echo 1 > /proc/sys/vm/drop_caches',
                    stdout=PIPE, stderr=PIPE,
                    shell=True).communicate())
        post_cache_mssg = "Memory usage after clearing RAM cache: {}%".format(
                            psutil.virtual_memory().percent)
        Popen("notify-send \"{}\"".format(post_cache_mssg), shell=True)
        print(post_cache_mssg)

        if psutil.virtual_memory().percent < RAM_USAGE_THRESHOLD:
            print("Clearing RAM cache saved the day")
            return
        # Kill top C{MAX_NUM_PROCESS_KILL} highest memory consuming processes.
        ps_killed_notify = ""
        for i, ps in enumerate(sorted(psutil.process_iter(),
                                      key=lambda x: x.memory_percent(),
                                      reverse=True)):
            # Do not kill root
            if ps.pid == 1:
                continue
            elif (i > MAX_NUM_PROCESS_KILL) or \
                    (psutil.virtual_memory().percent < RAM_USAGE_THRESHOLD):
                messagebox.showwarning('Killed proccess - save_hang',
                                       ps_killed_notify)
                Popen("notify-send \"{}\"".format(ps_killed_notify), shell=True)
                return
            else:
                try:
                    ps_killed_mssg = "Killed {} {} ({}) which was consuming {" \
                                     "} % memory (memory usage={})". \
                        format(i, ps.name(), ps.pid, ps.memory_percent(),
                               psutil.virtual_memory().percent)
                    ps.kill()
                    time.sleep(1)
                    ps_killed_mssg += "Current memory usage={}".\
                        format(psutil.virtual_memory().percent)
                    print(ps_killed_mssg)
                    ps_killed_notify += ps_killed_mssg + "\n"
                except Exception as err:
                    print("Error while killing {}: {}".format(ps.pid, err))
    else:
        print("Memory usage = " + str(psutil.virtual_memory().percent))
    root.update()


if __name__ == "__main__":
    while True:
        try:
            main()
        except Exception as err:
            print(err)
        time.sleep(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
