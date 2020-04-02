---
title: PIL example Image01 file search (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Image01 file search'


Modules used in program: 
* `import glob`

## python Image01 file search

Python PIL example: Image01 file search

```python
import glob
from pathlib import WindowsPath, Path

file = glob.glob("E:\Pictures\\2018\\." + '/**/*.jpg', recursive=True)

for i in file:
    n = Path(i).name
    p = str(Path(i).absolute())
    print(p[:])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
