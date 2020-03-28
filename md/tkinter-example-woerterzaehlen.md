---
title: tkinter example woerterzaehlen (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'woerterzaehlen'


Modules used in program: 
* `import re`

## python woerterzaehlen

Python tkinter example: woerterzaehlen

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
"""
Wörterzähler für Python 2.7 und 3.x
03.01.2014 MH Quelltext für Python 2.7 und 3.x
19.12.2013 MH Anzeige der Großbuchstaben
30.5.2013 MH Quelltext nach PEP8 überprüft
http://creativecommons.org/licenses/by-nc-sa/3.0/de/
"""

try:
    import Tkinter as tk
except ImportError:
    import tkinter as tk

try:
    import tkinter.scrolledtext as tkscrolledtext
except ImportError:
    import ScrolledText as tkscrolledtext

import re
from collections import Counter


class Application(tk.Frame):
    def Textanalysieren(self):
        """text analyse and display the analyse"""
        words = re.split('[-.,!\s]+',
         self.text.get(1.0, 'end').title()[:-1])

        self.wliste.config(state='normal')
        self.windex.config(state='normal')
        self.wliste.delete(1.0, 'end')
        self.windex.delete(1.0, 'end')

        word_counts = Counter(words)
        liste = ('{}\n\nAlphabetisch sortiert:\n{}'.format("\n".join(words),
         "\n".join(sorted(words))))
        index = ''.join('{} x {}\n'.format(word_counts[word], word)
         for word in sorted(word_counts))
        if words != ['']:
            self.wliste.insert('end', liste)
            self.windex.insert('end', index)
            self.lab["text"] = ("Der Text besteht aus {} Zeichen "
             "und aus {} Wörtern und enthält {} Großbuchstaben"
             .format(sum(c.isalpha() for c in self.text.get(1.0, 'end')),
             len(words), sum(c.isupper() for c in self.text.get(1.0, 'end'))))
        else:
            self.lab["text"] = "Kein Text vorhanden"
        self.windex.config(state='disabled')
        self.wliste.config(state='disabled')

    def createWidgets(self):
        """create GUI Tkinter"""
        self.QUIT = tk.Button(self)
        self.QUIT["text"] = "Ende"
        self.QUIT["fg"] = "red"
        self.QUIT["command"] = self.quit
        self.QUIT.pack({"side": "top", "fill": 'x'})

        self.lab = tk.Label(self, text="Viel Spaß mit dem Worterzählen")
        self.lab.pack({"side": "bottom"})

        self.analysieren = tk.Button(self)
        self.analysieren["text"] = "Text analysieren"
        self.analysieren["command"] = self.Textanalysieren
        self.analysieren["bg"] = "lightblue"
        self.analysieren.pack({"side": "bottom", "fill": 'x'})

        self.lab1 = tk.Label(self,
         text='Texteingabe:                                                   '
              '  Wörter-Liste:                         '
              '      Wörter-Index:')

        self.lab1.pack({"fill": "none"})
        self.lab1.pack({"side": "top"})
        self.lab1.pack({"fill": 'y'})

        #input text
        self.text = tkscrolledtext.ScrolledText(self)
        self.text["width"] = 50
        self.text["height"] = 20
        self.text.pack({"side": "left"})

        #word list in alphabet order
        self.wliste = tkscrolledtext.ScrolledText(self)
        self.wliste["width"] = 25
        self.wliste["height"] = 20
        self.wliste["state"] = 'disabled'
        self.wliste.pack({"side": "left"})

        #word index, time of the word in the text
        self.windex = tkscrolledtext.ScrolledText(self)
        self.windex["width"] = 25
        self.windex["height"] = 20
        self.windex["state"] = 'disabled'
        self.windex.pack({"side": "left"})

    def __init__(self, master=None):
        tk.Frame.__init__(self, master)
        self.pack(expand='yes')
        self.createWidgets()

root = tk.Tk()
root.title("Wörterzählen mit Python")
app = Application(master=root)
app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
