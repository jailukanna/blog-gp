---
title: tkinter example validate input tk (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'validate input tk'


Modules used in program: 
* `import tkinter as tk  # python 3.x`

## python validate input tk

Python tkinter example: validate input tk

```python
import tkinter as tk  # python 3.x
# import Tkinter as tk # python 2.x


class Example(tk.Frame):

    def __init__(self, parent):
        tk.Frame.__init__(self, parent)

        # valid percent substitutions (from the Tk entry man page)
        # note: you only have to register the ones you need; this
        # example registers them all for illustrative purposes
        #
        # %d = Type of action (1=insert, 0=delete, -1 for others)
        # %i = index of char string to be inserted/deleted, or -1
        # %P = value of the entry if the edit is allowed
        # %s = value of entry prior to editing
        # %S = the text string being inserted or deleted, if any
        # %v = the type of validation that is currently set
        # %V = the type of validation that triggered the callback
        #      (key, focusin, focusout, forced)
        # %W = the tk name of the widget

        vcmd = (self.register(self.onValidate),
                '%d', '%i', '%P', '%s', '%S', '%v', '%V', '%W')
        self.entry = tk.Entry(self, validate="key", validatecommand=vcmd)
        self.text = tk.Text(self, height=10, width=40)
        self.entry.pack(side="top", fill="x")
        self.text.pack(side="bottom", fill="both", expand=True)

    def onValidate(self, d, i, P, s, S, v, V, W):
        self.text.delete("1.0", "end")
        self.text.insert("end", "d='%s' type: %s\n" % (d, type(d)))
        self.text.insert("end", "i='%s' type: %s\n" % (i, type(i)))
        self.text.insert("end", "P='%s' type: %s\n" % (P, type(P)))
        self.text.insert("end", "s='%s' type: %s\n" % (s, type(s)))
        self.text.insert("end", "S='%s' type: %s\n" % (S, type(S)))
        self.text.insert("end", "v='%s' type: %s\n" % (v, type(v)))
        self.text.insert("end", "V='%s' type: %s\n" % (V, type(V)))
        self.text.insert("end", "W='%s' type: %s\n" % (W, type(W)))

        # Disallow anything but lowercase letters
        if S == S.lower():
            return True
        else:
            self.bell()
            return False


if __name__ == "__main__":
    root = tk.Tk()
    Example(root).pack(fill="both", expand=True)
    root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
