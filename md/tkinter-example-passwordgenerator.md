---
title: tkinter example passwordgenerator (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'passwordgenerator'

Functions in program: 
* `def generate_password(lenght_password, select_text):`

Modules used in program: 
* `import string`
* `import random`

## python passwordgenerator

Python tkinter example: passwordgenerator

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
"""
Passwordgenerator für Python
03.01.2014 MH Quelltext für Python 2.7 und 3.x
30.5.2013 MH Quelltext nach PEP8 überprüft
http://creativecommons.org/licenses/by-nc-sa/3.0/de/
"""

try:
    import Tkinter as tk
except ImportError:
    import tkinter as tk
import random
import string

string_seletion = (string.ascii_uppercase, string.ascii_lowercase,
                   string.digits, string.punctuation,
                   "äöüÖÜÄáÁŕŔźŹćĆéÉíÍîÎêÊôÔûÛâÂẑẐ",
                   "因此特別適合團隊作的寫作方式系統為這個社群提供了簡單的交流工具獲得相關參加國家大量電視節目的播映權")

def generate_password(lenght_password, select_text):
    """return a random string"""
    auswahl = ""
    for ic, e in zip(select_text,string_seletion):
        if ic:
            auswahl += e
    #random char in the lenght of value of the scrollbar
    return "".join(random.choice(auswahl) for _ in range(lenght_password))


class Application(tk.Frame):
    def print_value(self, val):
        if self.var1.get() + self.var2.get() + self.var3.get() \
         + self.var4.get() + self.var5.get() == 0 + self.var6.get() == 0:
            self.c1.select()
            self.var1.set(1)
        selection = (self.var1.get(), self.var2.get(), self.var3.get(),
         self.var4.get(), self.var5.get(), self.var6.get())
        text_password = generate_password(int(val), selection)
        self.entry.config(state='normal')
        self.entry.delete(0, 'end')
        self.entry.insert('end', text_password)

    def reset(self):
        self.slider.set(8)

    def move_scroll(self, *L):
        """move the scrollbar"""
        op, howMany = L[0], L[1]

        if op == "scroll":
            units = L[2]
            self.entry.xview_scroll(howMany, units)
        elif op == "moveto":
            self.entry.xview_moveto(howMany)

    def createWidgets(self):
        """GUI with Tkinter"""
        # beginn selection of password type
        self.lab1 = tk.Label(self, text="Auswahl:")
        self.lab1.pack({"fill": "none", "side": "top"})

        self.var1 = tk.IntVar()
        self.c1 = tk.Checkbutton(self, text="Großbuchstaben",
         variable=self.var1)
        self.c1.pack({"side": "top"})
        self.var2 = tk.IntVar()
        self.c2 = tk.Checkbutton(self, text="Kleinbuchstaben",
         variable=self.var2)
        self.c2.pack({"side": "top"})
        self.var3 = tk.IntVar()
        self.c3 = tk.Checkbutton(self, text="Zahlen",
         variable=self.var3)
        self.c3.pack({"side": "top"})
        self.var4 = tk.IntVar()
        self.c4 = tk.Checkbutton(self, text="Sonderzeichen",
         variable=self.var4)
        self.c4.pack({"side": "top"})
        self.var5 = tk.IntVar()
        self.c5 = tk.Checkbutton(self, text="Umlaute", variable=self.var5)
        self.c5.pack({"side": "top"})
        self.var6 = tk.IntVar()
        self.c6 = tk.Checkbutton(self, text="Chinesische Schriftzeichen",
         variable=self.var6)
        self.c6.pack({"side": "top"})

        #Scale for the lenght of the password
        self.slider = tk.Scale(self, from_=3, to=100,
                             orient='horizontal',
                             length="3i",
                             label="Passwortlänge",
                             command=self.print_value)
        self.slider.set(8)
        self.slider.pack(side='top', fill='x')

        #Reset Button to set 8 of the lenght
        self.reset = tk.Button(self, text='Reset', command=self.reset)
        self.reset.pack(side='top')

        #display the password
        self.entry = tk.Entry(self, width=50)
        self.entry["state"] = 'disabled'
        self.entry.pack({"side": "top", "fill": "x"})

        #to move the password in the entry
        self.entryScroll = tk.Scrollbar(self, orient='horizontal',
          command=self.move_scroll)
        self.entryScroll.pack({"side": "top", "fill": "x"})
        self.entry["xscrollcommand"] = self.entryScroll.set

        self.lab = tk.Label(self, text="Viel Spaß mit dem Programm")
        self.lab.pack({"side": "top"})

        #quit button
        self.QUIT = tk.Button(self)
        self.QUIT["text"] = "Ende"
        self.QUIT["fg"] = "red"
        self.QUIT["command"] = self.quit
        self.QUIT.pack({"side": "top", "fill": 'x'})

    def __init__(self, master=None):
        tk.Frame.__init__(self, master)
        self.pack(expand='YES')
        self.createWidgets()

root = tk.Tk()
root.title("Passwortgenerator mit Python")
app = Application(master=root)
app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
