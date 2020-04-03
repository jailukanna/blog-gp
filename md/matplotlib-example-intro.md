---
title: matplotlib example intro (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'intro'

Functions in program: 
* `def main():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import scipy as sp`
* `import pandas as pd`
* `import numpy as np`
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`

## python intro

Python matplotlib example: intro

```python
# Download Anaconda https://www.anaconda.com/download/
# Anaconda is a standalone python environment that doesn't mess with your system python and comes with useful libraries pre-installed
# execute anaconda
# cd ~/Downloads
# bash Anaconda3-5.3.1-Linux-x86_64.sh
# rm Anaconda3-5.3.1-Linux-x86_64.sh
# download visual studio code and start it up
# go to extensions tab (win linux: ctrl+shift+x)
# search for and download python extension for vscode
# search for and download jupyter extension for vscode
# Open the Command Palette (Command+Shift+P on macOS and Ctrl+Shift+P on Windows/Linux) and type in one of the following commands:
# Python: Select Interpreter
# Choose anaconda directory python
# restart visual studio code
# Copy and paste this in a file e.g. test.py and click Run Cell above #%%

# Useful reference
# https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf
# http://mcsp.wartburg.edu/zelle/python/graphics/graphics.pdf

#%%
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np

x = np.linspace(0, 20, 100)
plt.plot(x, np.sin(x))
plt.show()

#%%
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np

x = np.linspace(0, 20, 100)
plt.plot(x, x)
plt.show()

#%%
import matplotlib.pyplot as plt
import matplotlib as mpl
import numpy as np
import pandas as pd
import scipy as sp

df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/school_earnings.csv")

df

#%%
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
ts = pd.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))
ts = ts.cumsum()
ts.plot()

#%%
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=list('ABCD'))
df['e'] = pd.Series(np.random.randn(1000), index=df.index)
df['f'] = pd.Series(np.linspace(0, 0.10, 1000), index=df.index)

df.at['2001-01-27', 'f'] = 100.00

df = df.cumsum()

df['g'] = df['A'].rolling(30).mean()

plt.figure() 
df.plot()
plt.legend(loc='best')

#%%
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# https://www.nasdaq.com/symbol/csv/historical
df = pd.read_csv('/home/carolosf/Downloads/HistoricalQuotes.csv')

# df.index = df['date']
df.index = pd.to_datetime(df['date'],infer_datetime_format=True)
df = df.drop('date', axis=1)
df['ma'] = df['open'].rolling(30).mean()
df.plot(figsize=(20,10))
#d = df['date']

#%%
# need to do in ~/anaconda3/bin: ./pip install graphics.py
from graphics import *
def main():
    win = GraphWin("My Circle", 1024, 768)
    c = Circle(Point(50,50), 10)
    c.draw(win)
    
    for i in range(0, 100):
        c = Circle(Point(100,i), 10)
        c.draw(win)
    
    
    win.getMouse() # pause for click in window
    win.close()
main()

#%%
from bokeh.io import push_notebook, show, output_notebook
from bokeh.layouts import row, gridplot
from bokeh.plotting import figure, show, output_file
from bokeh.models import ColumnDataSource
output_notebook()

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# https://www.nasdaq.com/symbol/csv/historical
df = pd.read_csv('/home/carolosf/Downloads/HistoricalQuotes.csv')

# df.index = df['date']
df.index = pd.to_datetime(df['date'],infer_datetime_format=True)
df = df.drop('date', axis=1)
df['ma'] = df['open'].rolling(30).mean()
df.plot(figsize=(20,10))
x = np.linspace(0, 4*np.pi, 100)
y = np.sin(x)

TOOLS = "pan,wheel_zoom,box_zoom,reset,save,box_select"

p1 = figure(title="Legend Example", tools=TOOLS, x_axis_type="datetime")
source = ColumnDataSource(data=df)
# p1.circle(y='open', x='date', source=source)
p1.line(y='close', x='date', color='navy', alpha=0.5, source=source)
p1.line(y='ma', x='date', color='red', alpha=0.5, source=source)

show(p1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
