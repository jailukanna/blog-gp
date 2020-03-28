---
title: tkinter example star (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'star'


Modules used in program: 
* `import sys`

## python star

Python tkinter example: star

```python
import sys

py_major, py_minor = sys.version_info[:2]

if py_major <= 2:
    import Tkinter as tk
else:
    import tkinter as tk

data_star = '''
R0lGODlhFAAUAPYAACIRACocADwzAEY+AE48AEE8DEw8C00+HVNDD0pEEEdBHVNHH2tjGFtPKVtU
JktBN2VSJ2pZJWhWLn5sIHhxIGtjPHpqNn9yRot/N4d5VIV7XYB5aYOAMZaGO6ebU7mkS4mHYJaL
b6CUYq2cZL6tZLSicrarcrKmfNTMauTMbODRbKmfhq6pi6ujkLqxqMa4k8C2ndTRgtrQi9XEltPG
mtPImtzNnuDOgu7YgubWiebfmf/7je/nnvTsl/fsnf/4ndLFo9rIpNzUq9PNt9TMueLToezdpujd
r+LRteTatebdvvfmofTpqfzwpP/8pf7yqv79q+vgs/ThtvbvuP7/s//3v//8uuTexOvdwOPeyOjj
zfrowv/3wf/8w//9y/Xr0vDn2Pr20f//1v//3/fv4v/+5P/96//+9P7+/QAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5
BAAAAAAAIf8LTUdLOEJJTTAwMDBoOEJJTQQEAAAAAABcHAJnAFA1MzYxNmM3NDY1NjQ1ZjVmNzdi
Y2FkMTkxZDAxODI0OTk2N2Y5ZjNmODFlNjU0OTlkZGMzMzk4N2FkMjczYjg5NzY3MDg3YTlmMGM3
YjA0MxwCAAACAAQAIf8LTUdLSVBUQzAwMDBcHAJnAFA1MzYxNmM3NDY1NjQ1ZjVmNzdiY2FkMTkx
ZDAxODI0OTk2N2Y5ZjNmODFlNjU0OTlkZGMzMzk4N2FkMjczYjg5NzY3MDg3YTlmMGM3YjA0MxwC
AAACAAQALAAAAAAUABQAAAfqgGiCg4JnhGhnhoeLZ2AuhYuRZ2ZYF2WKkYtlQQ+XmYxYUg5AmJmJ
aGYvXiMXhq5mgmZlZrKzZSddPAKztGOzZ1paSklCNTQ2XVZQHCY1NkFHRy5EZy0ZRkwqOU1UVFBQ
KCopODIJGmWIWAYkTu3eUE478h8CQrODZRYd7U7f8D8TIsA6dKaMCQr9/DlhIAKdpDEx2n1z0mOA
Q0lldEB58sQHvAFZTJWJwoVLCQAESEDxEKJUITNiiniREADJFgQcmBwYeAjmlAIHXJGp0KDBF0lf
CoC4iIhFgBUuBW0YwrNQoweEME2S5DIQADs=
'''

class App(tk.Frame, object):
    def __init__(self, parent=None, *args, **kwargs):
        super(App, self).__init__(parent, *args, **kwargs)

        self.parent = parent
        self.image_star = tk.PhotoImage(data=data_star)
        self.id_animation = None
        self.star_i, self.star_j = 0, 0

        self.frame_board = tk.Frame(self)
        self.frame_control = tk.Frame(self)
        self.frame_board.pack()
        self.frame_control.pack()

        # frame_board
        self.canvas_board = tk.Canvas(
                self.frame_board,
                width=400, height=300,
                scrollregion=(0, 0, 1500, 1500),
                bg='white',
                highlightthickness=0
        )

        self.scroll_board_x = tk.Scrollbar(
                self.frame_board,
                orient=tk.HORIZONTAL
        )

        self.scroll_board_y = tk.Scrollbar(
                self.frame_board,
                orient=tk.VERTICAL
        )

        self.canvas_board.config(xscrollcommand=self.scroll_board_x.set)
        self.canvas_board.config(yscrollcommand=self.scroll_board_y.set)
        self.scroll_board_x.config(command=self.canvas_board.xview)
        self.scroll_board_y.config(command=self.canvas_board.yview)

        # scrolls should be packed before the canvas
        self.scroll_board_x.pack(side=tk.BOTTOM, fill=tk.X)
        self.scroll_board_y.pack(side=tk.RIGHT, fill=tk.Y)
        self.canvas_board.pack(side=tk.LEFT, expand=tk.TRUE, fill=tk.BOTH)

        # frame_control
        self.button_run = tk.Button(
                self.frame_control,
                text='Run',
                width=8,
                command=self.cb_run
        )

        self.button_stop = tk.Button(
                self.frame_control,
                text = 'Stop',
                width=8,
                command=self.cb_stop
        )

        self.button_run.grid(row=0, column=0, padx=10, pady=5)
        self.button_stop.grid(row=0, column=1, padx=10, pady=5)

    def cb_run(self):
        if self.id_animation is None:
            self.do_animation()

    def cb_stop(self):
        # stop the animation
        if self.id_animation is not None:
            self.after_cancel(self.id_animation)
            self.id_animation = None

        # clear the canvas
        self.canvas_board.delete(tk.ALL)

        # reset the coordinates
        self.star_i, self.star_j = 0, 0

    def do_animation(self):
        self.draw_star(self.star_i, self.star_j)

        if self.star_j >= self.star_i: # new line
            self.star_i += 1
            self.star_j = 0
        else:
            self.star_j += 1

        self.id_animation = self.after(200, self.do_animation)

    def draw_star(self, i, j):
        x = 20 + j * 30
        y = 20 + i * 30

        self.canvas_board.create_image((x, y), image=self.image_star)

if __name__ == '__main__':
    root = tk.Tk()
    root.title('Star')
    app = App(root)
    app.pack()
    root.mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
