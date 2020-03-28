---
title: tkinter example progressbar threading (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'progressbar threading'

Functions in program: 
* `def click(pgbar):`
* `def proceso(msg, pgbar):`

Modules used in program: 
* `import time`
* `import tkinter.messagebox as mbox`
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python progressbar threading

Python tkinter example: progressbar threading

```python
import tkinter as tk
import tkinter.ttk as ttk
import tkinter.messagebox as mbox
import time
from threading import *

proceso_thread = None
cuenta = 0


def proceso(msg, pgbar):
    global cuenta
    print(msg, cuenta)
    time.sleep(5)
    pgbar.stop()
    pgbar.grid_forget()
    print('Termino')


def click(pgbar):
    global proceso_thread
    global cuenta
    if not proceso_thread or not proceso_thread.isAlive():
        pgbar.grid(row=1, column=0, sticky='we', pady=10)
        pgbar.start(15)
        proceso_thread = Thread(target=proceso, args=[
                                'Iniciando proceso', pgbar])
        proceso_thread.setDaemon(True)
        cuenta += 1
        proceso_thread.start()
    else:
        mbox.showwarning('Error', 'El proceso se esta ejecutando aun')


if __name__ == "__main__":
    app = tk.Tk()
    app.title('Test progress bar')
    frm_app = tk.Frame(app)
    frm_app.grid(padx=10, pady=10)
    app.grid_rowconfigure(0, weight=1)
    app.grid_columnconfigure(0, weight=1)
    s = ttk.Style()
    s.theme_use('default')
    s.configure('TProgressbar', background='green')
    lbl_msg = tk.Label(frm_app, text='Por favor, espere...')
    lbl_msg.grid(row=0, column=0, sticky='we')
    pb_wait = ttk.Progressbar(
        frm_app, orient=tk.HORIZONTAL, length=200, mode='indeterminate')
    btn_start = tk.Button(frm_app, text='Iniciar',
                          command=lambda: click(pb_wait))
    btn_start.grid(row=2, column=0, sticky='we')
    app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
