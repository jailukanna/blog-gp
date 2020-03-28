---
title: tkinter example HW7.2-Partho (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'HW7.2-Partho'

Functions in program: 
* `def HW5_Way():`
* `def NumPy_SciPy_Way():`

Modules used in program: 
* `import Tkinter, tkFileDialog, csv, sys, numpy as np, timeit`

## python HW7.2-Partho

Python tkinter example: HW7.2-Partho

```python
# MSDA IS602 Homework Week 7
# Author: Partho
# Date: 10/23/2013

import Tkinter, tkFileDialog, csv, sys, numpy as np, timeit
from scipy.optimize import curve_fit
from scipy import sqrt

# Hide Tk root window
root = Tkinter.Tk()
root.withdraw()

print
print("Home work submitted by Partho for Week 7 - 2. Comparison")
print("========================================================")
print

# Read datafile name with path - this is following question #1
input_file = "C:\\Users\\Partho\\Documents\\IS602\\W5\\brainandbody.csv"

def NumPy_SciPy_Way():
    # Read file
    rec_data = np.genfromtxt(input_file, dtype=None, names=['indx','br_wt','bd_wt'], delimiter=',', skip_header=1)
    rec_no = len(rec_data)

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

def HW5_Way():
    # Read file
    br_wt = []
    bd_wt = []
    with open(input_file, "r") as fl:
        allrecs = csv.reader(fl)
        next(allrecs, None)             # Skip header
        for row in allrecs:
            br_wt.append(float(row[1]))
            bd_wt.append(float(row[2]))
    N = len(br_wt)
    if N > 0:
        # Calculate correlation coefficient
        sumX = sum(br_wt)
        sumY = sum(bd_wt)
        sumXY = sum(br_wt[i] * bd_wt[i] for i in range(N))
        sumX2 = sum(br_wt[i]**2 for i in range(N))
        sumY2 = sum(bd_wt[i]**2 for i in range(N))
        r = (N * sumXY - sumX * sumY) / (sqrt(N*sumX2 - (sumX ** 2)) * sqrt(N*sumY2 - (sumY ** 2)))
        # Calculate mean and standard deviation for "Brain Weight"
        mnx = sum(br_wt)/N
        sx2 = sum((br_wt[i] - mnx)**2 for i in range(N))
        sdx = sqrt(sx2/N)

        # Calculate mean and standard deviation for "Body Weight"
        mny = sum(bd_wt)/N
        sy2 = sum((bd_wt[i] - mny)**2 for i in range(N))
        sdy = sqrt(sy2/N)

        # Calculate slope (b) and intercept (a)
        b = r * sdy/sdx
        a = mny - b * mnx

itr = 10000

t1 = timeit.timeit(HW5_Way, number=itr)
print("Regression calculation using built in python: %i loops = %f seconds" % (itr, t1))

t2 = timeit.timeit(NumPy_SciPy_Way, number=itr)
print("Regression calculation using numpy/scipy: %i loops = %f seconds" % (itr, t2))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
