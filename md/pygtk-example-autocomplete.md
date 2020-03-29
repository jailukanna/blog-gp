---
title: pygtk example autocomplete (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'autocomplete'

Functions in program: 
* `def main():`

Modules used in program: 
* `import gtk`
* `import pygtk`

## python autocomplete

Python pygtk example: autocomplete

```python
#!/usr/bin/env python

import pygtk
pygtk.require('2.0')
import gtk

class CompletitionEntry:
    def __init__(self):
        window = gtk.Window()
        window.connect('destroy', lambda w: gtk.main_quit())
        entry = gtk.Entry()
        window.add(entry)
        completion = gtk.EntryCompletion()
        self.liststore = gtk.ListStore(str)
        for distro in ['debian', 'ubuntu', 'fedora', 'knoppix', 'suse',
                  'arch', 'gentoo']:
            self.liststore.append([distro])
        completion.set_model(self.liststore)
        entry.set_completion(completion)
        completion.set_text_column(0)
        entry.connect('activate', self.autocompletamento)
        window.show_all()
        return

    def autocompletamento(self, entry):
        text = entry.get_text()
        if text:
            if text not in [row[0] for row in self.liststore]:
                self.liststore.append([text])
                entry.set_text('')
        return

def main():
    gtk.main()
    return

if __name__ == "__main__":
    provaentry = CompletitionEntry()
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
