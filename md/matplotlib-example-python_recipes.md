---
title: matplotlib example python recipes (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'python recipes'


## python python recipes

Python matplotlib example: python recipes

```python
#%% Import ipython and adjust for qt5 backend, keeping compatibility with non-ipython envs
from contextlib import suppress
with suppress(ImportError):
    from IPython import get_ipython
with suppress(AttributeError, NameError):
    # List available APIs
    get_ipython().run_line_magic("matplotlib", "-l")
    get_ipython().run_line_magic("matplotlib", "qt5")

    
#%% Adjust matplotlib qt5 backend for high DPI monitors
if plt.get_backend() == "Qt5Agg":
    from matplotlib.backends.qt_compat import QtWidgets  # pylint: disable=C0412
    from sys import argv

    qApp = QtWidgets.QApplication(argv)
    plt.matplotlib.rcParams["figure.dpi"] = qApp.desktop().physicalDpiX()
plt.rcParams["figure.autolayout"] = True


#%% Adjust figure default size
plt.rcParams["figure.figsize"] = (12, 8)  # Default fig size (w, h))


#%% Ugly hack to allow absolute import from the root folder
# whatever its name is. Please forgive the heresy.
inDebugging = True
if (__name__ == "__main__" and __package__ is None) or inDebugging:
    from sys import path
    from os.path import dirname

    path.append(dirname(path[0]))
    __package__ = "txPBF"  # Name of the subfolder where this script is


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
