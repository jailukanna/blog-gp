---
title: tkinter example HW7.1-Partho (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'HW7.1-Partho'


Modules used in program: 
* `import Tkinter, tkFileDialog, csv, sys, numpy as np`

## python HW7.1-Partho

Python tkinter example: HW7.1-Partho

```python
# MSDA IS602 Homework Week 7
# Author: Partho
# Date: 10/23/2013

import Tkinter, tkFileDialog, csv, sys, numpy as np
from scipy.optimize import curve_fit

# Hide Tk root window
root = Tkinter.Tk()
root.withdraw()

print
print("Home work submitted by Partho for Week 7 -  1. Regression calculation using SciPy")
print("=================================================================================")
print
print("[This program assumes that the datafile has a Header]\n")
print("Per Homework instruction, we need to find the model in the form:")
print("\t\tbo = X * br + Y")
print

try:
    # Read datafile name with path - this is following question #1
    input_file = tkFileDialog.askopenfilename()
    #input_file = "C:\\Users\\Partho\\Documents\\IS602\\W5\\brainandbody.csv"

    # Read file
    rec_data = np.genfromtxt(input_file, dtype=None, names=['indx','br_wt','bd_wt'], delimiter=',', skip_header=1)
    rec_no = len(rec_data)
    print("Total record(s) in datafile %s: %d" % (input_file, rec_no))
    print

    if rec_no > 0:
        # Separating the data
        br_wt = rec_data['br_wt']
        bd_wt = rec_data['bd_wt']

        # Creating a function to model
        def func(x, a, b):
            return a * x + b

        # Executing curve_fit on input data, popt returns the optimal values for the parameters
        # so that the sum of the squared error of f(xdata, *popt) - ydata is minimized
        popt, pcov = curve_fit(func, br_wt, bd_wt)
        print("Regression formula:\n\t\t bo = %f * br + %f" % (popt[0], popt[1]))
    else:
        print("Datafile has no data in it, Exiting...")
except IOError:
    # User presses Cancel button instead of selecting a file
    print("Sorry, you have aborted File selection option!!!")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
