---
title: matplotlib example matplotlib show chinese (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib show chinese'

Functions in program: 
* `def plt_show_ch():`
* `def get_all_text(obj):`

Modules used in program: 
* `import matplotlib.pyplot as plt `
* `import matplotlib`

## python matplotlib show chinese

Python matplotlib example: matplotlib show chinese

```python
# matplotlib無法顯示中文字的問題
import matplotlib
import matplotlib.pyplot as plt 
from matplotlib import font_manager
microhei = matplotlib.font_manager.FontProperties(fname='wqy-microhei.ttc')

def get_all_text(obj):
    ''' 
    Get all text objects in a matplotlib Figure
    Helper for plt_show_ch
    '''
    queue = [obj]
    all_text = []
    while queue:
        currobj = queue.pop(0)
        if isinstance(currobj, matplotlib.text.Text):
            all_text.append(currobj)
        else:
            queue = queue + currobj.get_children()
    return all_text

def plt_show_ch():
    '''代替 plt.show()
    在 show 之前先把所有 Text 物件設定好用中文字體
    '''
    fig = plt.gcf()
    for textobj in get_all_text(fig):
        fontsize = textobj.get_fontsize()
        textobj.set_fontproperties(microhei)
        textobj.set_fontsize(fontsize)
    return plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
