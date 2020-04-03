---
title: matplotlib example gnome-sysmon-cpu-gradient (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'gnome-sysmon-cpu-gradient'


Modules used in program: 
* `import argparse`

## python gnome-sysmon-cpu-gradient

Python matplotlib example: gnome-sysmon-cpu-gradient

```python
import argparse
from matplotlib.colors import to_hex
from matplotlib.pyplot import get_cmap
from matplotlib.cm import cmap_d
from numpy import linspace

parser = argparse.ArgumentParser(
    description='Generate color gradient for GNOME System Monitor. '
    'Can be set at dconf key /org/gnome/gnome-system-monitor/cpu-colors'
)
parser.add_argument('cores', help='number of cores', type=int)
parser.add_argument('--colormap', help=f'matplotlib colormap: {list(cmap_d.keys())}', default='rainbow', type=str)
parser.add_argument('--start', help="colormap start value (between 0 and 1)", default=0.0, type=float)
parser.add_argument('--end', help="colormap end value (between 0 and 1)", default=1.0, type=float)

if __name__=="__main__":
    args = parser.parse_args()

    # Choose a colormap from https://matplotlib.org/3.1.0/tutorials/colors/colormaps.html
    map = get_cmap(args.colormap)

    # Start/end values to select part of the colormap.
    #   The first color in the gradient will always be visible in the graph
    #   -> e.g.: for 'rainbow' I set the extent to [0.2, 1] to make the first color blue instead of purple
    cpu_colors = [
        (i, to_hex(map(value)))
        for i,value in enumerate(linspace(args.start,args.end,args.cores))
    ]

    print(cpu_colors)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
