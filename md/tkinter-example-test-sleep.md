---
title: tkinter example test-sleep (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'test-sleep'

Functions in program: 
* `def main():`
* `def worker():`

Modules used in program: 
* `import tkinter.scrolledtext as ScrolledText`
* `import tkinter as tk`
* `import time`
* `import _thread as thread`
* `import threading`
* `import os`

## python test-sleep

Python tkinter example: test-sleep

```python
#! /usr/bin/env python3

import os
import threading
import _thread as thread
import time
import tkinter as tk
import tkinter.scrolledtext as ScrolledText

class myGUI(tk.Frame):

    # This class defines the graphical user interface 
    
    def __init__(self, parent, *args, **kwargs):
        tk.Frame.__init__(self, parent, *args, **kwargs)
        self.root = parent
        self.build_gui()
        
    def build_gui(self):                    
        # Build GUI
        self.root.title('TEST')
        self.root.option_add('*tearOff', 'FALSE')
        self.grid(column=0, row=0, sticky='ew')
        self.grid_columnconfigure(0, weight=1, uniform='a')

        # Add text widget to display logging info
        st = ScrolledText.ScrolledText(self, state='disabled')
        st.configure(font='TkFixedFont')
        st.grid(column=0, row=1, sticky='w', columnspan=4)

def worker():
    """Skeleton worker function, runs in separate thread (see below)"""  

    # Print some text to console
    print("Working!")
    # Wait 2 seconds to avoid race condition
    time.sleep(2)
    # This triggers a KeyboardInterrupt in the main thread
    thread.interrupt_main()
        
def main():
    try:
        root = tk.Tk()
        myGUI(root)
        t1 = threading.Thread(target=worker, args=[])
        t1.start()
        root.mainloop()
        t1.join()
    except KeyboardInterrupt:
        # Close program if subthread issues KeyboardInterrupt
        os._exit(0)
    
main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
