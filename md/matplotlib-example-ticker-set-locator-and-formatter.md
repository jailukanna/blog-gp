---
title: matplotlib example ticker-set-locator-and-formatter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ticker-set-locator-and-formatter'

Functions in program: 
* `def format_func(value, tick_number):`

## python ticker-set-locator-and-formatter

Python matplotlib example: ticker-set-locator-and-formatter

```python
# from https://jakevdp.github.io/PythonDataScienceHandbook/04.10-customizing-ticks.html
fig, ax = plt.subplots()
x = np.linspace(0, 3 * np.pi, 1000)
ax.plot(x, np.sin(x), lw=3, label='Sine');
ax.plot(x, np.cos(x), lw=3, label='Cosine');

# Set up grid, legend, and limits
ax.grid(True)
ax.legend(frameon=False)

# xlim = ylim
ax.axis('equal')
ax.set_xlim(0, 3 * np.pi);

# set locator to be product of pi/2
ax.xaxis.set_major_locator(plt.MultipleLocator(np.pi/2));
ax.xaxis.set_minor_locator(plt.MultipleLocator(np.pi/4));

# mapping xticks to Latex
def format_func(value, tick_number):
    # find number of multiples of pi/2
    N = int(np.round(2 * value / np.pi))
    if N == 0:
        return "0"
    elif N == 1:
        return r"$\pi/2$"
    elif N == 2:
        return r"$\pi$"
    elif N % 2 > 0:
        return r"${0}\pi/2$".format(N)
    else:
        return r"${0}\pi$".format(N // 2)
    
# use plt.FuncFormatter to format xticks
ax.xaxis.set_major_formatter(plt.FuncFormatter(format_func));

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
