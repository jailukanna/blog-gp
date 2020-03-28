---
title: tkinter example thread safe non blocking tk prompt (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'thread safe non blocking tk prompt'

Functions in program: 
* `def prompt_thread_loop(master):`
* `def prompt_start_thread(func, args=(), kwargs={}):`
* `def prompt(title='', message='', message_type='INFO'):`

Modules used in program: 
* `import threading`
* `import tkinter.messagebox as tkMessageBox`
* `import tkinter as tk`

## python thread safe non blocking tk prompt

Python tkinter example: thread safe non blocking tk prompt

```python
import tkinter as tk
import tkinter.messagebox as tkMessageBox
import threading
from queue import Queue

# Thread-safe version.
# Tkinter functions are put into queue and called by prompt_thread_loop
# in the main thread.

messagebox_queue = Queue()


def prompt(title='', message='', message_type='INFO'):
    if message_type == 'INFO':
        messagebox_queue.put((tkMessageBox.showinfo, (title, message), {}))
    elif message_type == 'ERROR':
        messagebox_queue.put((tkMessageBox.showerror, (title, message), {}))


def prompt_start_thread(func, args=(), kwargs={}):
    threading.Thread(target=func, args=args, kwargs=kwargs).start()


def prompt_thread_loop(master):
    try:
        while True:
            func, args, kwargs = messagebox_queue.get_nowait()
            func(*args, **kwargs)
    except:
        pass
    master.after(100, lambda: prompt_thread_loop(master))

if __name__ == '__main__':
    root = tk.Tk()
    tkMessageBox.showinfo(message='blocking prompt')
    prompt_thread_loop(root)  # prompt_thread_loop is launched here
    prompt_start_thread(lambda: prompt(message='non blocking prompt'))
    label = tk.Label(root, text='code after the second prompt.')
    label.pack()
    root.mainloop()

    # 循环查询有无弹窗：
    # prompt_thread_loop(root)
    # 用法：
    # from view import prompt_start_thread, prompt, view
    # prompt_start_thread(lambda: prompt(message='asdf', master=view))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
