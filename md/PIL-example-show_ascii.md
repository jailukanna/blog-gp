---
title: PIL example show ascii (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'show ascii'

Functions in program: 
* `def show_ascii(w=200, h=50, chars=' .\',;"oO%8@#'):`

Modules used in program: 
* `import tempfile`
* `import PIL`

## python show ascii

Python PIL example: show ascii

```python
# coding: utf-8

import PIL
import tempfile

def show_ascii(w=200, h=50, chars=' .\',;"oO%8@#'):
    from pylab import tight_layout, savefig
    tight_layout()
    with tempfile.NamedTemporaryFile(suffix='.png') as f:
        savefig(f.name)
        im = PIL.Image.open(f.name).resize((w, h)).convert("L")
    print('\n'.join([''.join([chars[11-im.getpixel((j, i))/22] for j in range(im.size[0])]) for i in range(im.size[1])]))

if __name__ == '__main__':

    # prevent a figure from popping up
    import matplotlib as mpl
    mpl.use('Agg')

    from pylab import *
    x = r_[:12:100j]
    plot(x, sin(x))
    show_ascii()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
