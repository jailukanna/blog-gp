---
title: pil example Image01 eloszlas (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Image01 eloszlas'

Functions in program: 
* `def main():`
* `def print_rows(conn):`
* `def db():`

Modules used in program: 
* `import matplotlib.cbook as cbook`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import img_db as idb`

## python Image01 eloszlas

Python pil example: Image01 eloszlas

```python
import img_db as idb
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.cbook as cbook

def db():

    return idb.open_db()

def print_rows(conn):
    rows = idb.read_tbl(conn,'ddd')
    for row in rows:
        fn = np.array([row[0]])
        val = np.array([row[1]])
        print(fn,'-',val)
        l = plt.scatter(fn,val)

    plt.show(l)

def main():
    conn = db()
    print_rows(conn)
    conn.close()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
