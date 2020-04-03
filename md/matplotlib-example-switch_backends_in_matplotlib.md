---
title: matplotlib example switch backends in matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'switch backends in matplotlib'


Modules used in program: 
* `import matplotlib`
* `import matplotlib`

## python switch backends in matplotlib

Python matplotlib example: switch backends in matplotlib

```python
# This file was obtained from StackOverflow:
#
# How to switch backends in matplotlib / Python
# https://stackoverflow.com/questions/3285193/how-to-switch-backends-in-matplotlib-python


import matplotlib
gui_env = ['TKAgg','GTKAgg','Qt4Agg','WXAgg']
for gui in gui_env:
    try:
        print("testing", gui)
        matplotlib.use(gui,warn=False, force=True)
        from matplotlib import pyplot as plt
        break
    except:
        continue
print("Using:",matplotlib.get_backend())


import matplotlib
gui_env = [i for i in matplotlib.rcsetup.interactive_bk]
non_gui_backends = matplotlib.rcsetup.non_interactive_bk
print(("Non Gui backends are:", non_gui_backends))
print(("Gui backends I will test for", gui_env))
for gui in gui_env:
    print(("testing", gui))
    try:
        matplotlib.use(gui,warn=False, force=True)
        from matplotlib import pyplot as plt
        print(("    ",gui, "Is Available"))
        plt.plot([1.5,2.0,2.5])
        fig = plt.gcf()
        fig.suptitle(gui)
        plt.show()
        print(("Using ..... ",matplotlib.get_backend()))
    except:
        print(("    ",gui, "Not found"))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
