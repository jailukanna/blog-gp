---
title: matplotlib example wot wn8 line graph (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'wot wn8 line graph'

Functions in program: 
* `def _quit():`
* `def on_key_event(event):`

Modules used in program: 
* `import tkinter as Tk`
* `import pprint`
* `import pandas as pd`
* `import matplotlib`

## python wot wn8 line graph

Python matplotlib example: wot wn8 line graph

```python
#!/usr/bin/env python3
# Plots tank damage per tier/level
#   Each dot is mean of tank types in that tier
#   Each tank type is a seperate line graph
# Inspired from here https://matplotlib.org/examples/user_interfaces/embedding_in_tk.html
# Good example of manipulating dataframes and creating stand alone application with Tk.
import matplotlib
matplotlib.use('TkAgg')

from numpy import arange, sin, pi
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
# implement the default mpl key bindings
from matplotlib.backend_bases import key_press_handler
import pandas as pd
import pprint

from matplotlib.figure import Figure
import tkinter as Tk

pp = pprint.PrettyPrinter( indent=4, compact=True )

root = Tk.Tk()
root.wm_title("Embedding in TK")

# Copied table from here into .tsv
df=pd.read_csv('/home/smeckley/examples/py/dataframe/wot_wn8_data.tsv', sep='\t')

# Drop data that is most likely manufactured (mutiple floats are whole numbers)
idx = df[ ( df['Dmg'].map(lambda x: x.is_integer()) ) & ( df['Win'].map(lambda x: x.is_integer()) )].index
df.drop( idx , inplace=True)

pp.pprint( df[df.Tier == 10] )
pp.pprint( df[df.Type == "Heavy Tanks"] )
pp.pprint( df[ (df['Type'] == "Heavy Tanks") & (df['Tier'] == 10) ] )
pp.pprint( df[ (df['Tier'] == 10) ].groupby('Type').Dmg.mean() )
df10=df[ (df['Tier'] == 10) ]
pp.pprint( df10 )
pp.pprint( df10.groupby('Type').Dmg.mean() )

df_tiers = [x for _, x in df.groupby(df['Tier'])]
for df_tier in df_tiers:
    pp.pprint( df_tier.groupby('Type').Dmg.mean() )


pp.pprint( df10.groupby('Type').Dmg.mean() )
df.set_index('Tier')


fig = Figure(figsize=(5, 4), dpi=100)
ax = fig.add_subplot(111)
ax.set_yscale('log')

df_types = [x for _, x in df.groupby(df['Type'])]
for df_type in df_types:
    pp.pprint( df_type.groupby('Tier').Dmg.mean() )
    ax.plot(df_type.groupby('Tier').Dmg.mean())

#df.plot(x='Tier', y='Dmg', ax=ax )
#ax.plot(t, data)
'%.100f' % (1.64%.1)


# ax tk.DrawingArea
canvas = FigureCanvasTkAgg(fig, master=root)
canvas.show()
canvas.get_tk_widget().pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)

toolbar = NavigationToolbar2TkAgg(canvas, root)
toolbar.update()
canvas._tkcanvas.pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)


def on_key_event(event):
    print('you pressed %s' % event.key)
    key_press_handler(event, canvas, toolbar)

canvas.mpl_connect('key_press_event', on_key_event)


def _quit():
    root.quit()     # stops mainloop
    root.destroy()  # this is necessary on Windows to prevent
                    # Fatal Python Error: PyEval_RestoreThread: NULL tstate

button = Tk.Button(master=root, text='Quit', command=_quit)
button.pack(side=Tk.BOTTOM)

Tk.mainloop()
# If you put root.destroy() here, it will cause an error if
# the window is closed with the window manager.


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
