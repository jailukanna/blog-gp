---
title: tkinter example getimages (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'getimages'

Functions in program: 
* `def main():`

Modules used in program: 
* `import tkinter.ttk`
* `import tkinter`
* `import threading`
* `import sys`
* `import re`
* `import queue`

## python getimages

Python tkinter example: getimages

```python
#!/usr/bin/env python3

from os.path import basename
from urllib.request import (urlopen, urlretrieve)
import queue
import re
import sys
import threading
import tkinter
import tkinter.ttk


class Downloader:

    _done = None
    _progress = None
    _thread = None

    def __init__(self, progress, done):
        self._progress = progress
        self._done = done
        self._thread = None

    def run(self, url):
        self._thread = threading.Thread(target=self._target)
        self._progress['maximum'] = 10
        self._progress['value'] = 0

    def join(self):
        if self._thread:
            self._thread.join()

    def _target(self):
        import time
        for i in range(10)
        time.sleep(1)


class Window:

    _window = None
    _downloader = None
    _progress = None
    _tasks = None

    def __init__(self):
        # Build UI
        self._window = tkinter.Tk()
        tkinter.Label(self._window, text='Въведи УРЛ').pack(pady=2)
        entry = tkinter.Entry(self._window)
        entry.bind('<Return>', self._download)
        entry.pack(pady=2)
        self._progress = tkinter.ttk.Progressbar(self._window)
        self._progress.pack(pady=2)
        # self._progress['maximum'] = 10
        # self._progress['value'] = 0

        # Start inner loop
        self._tasks = queue.Queue()
        self._inner()

    def enqueue(self, task, args):
        self._tasks.put((task, args))

    def join(self):
        pass

    def mainloop(self):
        tkinter.mainloop()

    def _inner(self):
        while 1:
            try:
                (fn, args) = self._tasks.get(False)
            except queue.Empty:
                break
            else:
                fn(*args)
        self._window.after(100, self._inner)

    def _download(self, event):
        url = event.widget.get()
        self._downloader = Downloader(url, self._done)

    def _done(self):
        pass
        # self.enqueue(
        # print('url={}'.format(url))
        # print(type(url))
        # print('DOWNLOAD!, self={}, event={}'.format(self, event.widget.get()))
        # event.widget.configure(state='disable')
        # self._progress['value'] += 1


def main():
    w = Window()
    w.mainloop()
    sys.exit(0)

    if len(sys.argv) <= 1:
        return 1
    contents = urlopen(sys.argv[1]).read()
    pattern = re.compile(
        b'<a class="projector_medium_image".*?href="([^"]+)"'
    )
    for url in pattern.findall(contents):
        utf = url.decode('utf-8')
        print('Downloading {}'.format(utf))
        urlretrieve(utf, basename(utf))


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
