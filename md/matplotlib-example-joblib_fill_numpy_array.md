---
title: matplotlib example joblib fill numpy array (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'joblib fill numpy array'

Functions in program: 
* `def assign_row(_output, i):`

Modules used in program: 
* `import tempfile`
* `import numpy as np`
* `import os`
* `import shutil`

## python joblib fill numpy array

Python matplotlib example: joblib fill numpy array

```python
"""
Demo based on joblib documentation to fill a Numpy array in parallel
This only works for newer version of joblib https://github.com/joblib/joblib
"""

import shutil
import os
import numpy as np

import tempfile
from joblib import Parallel, delayed
from joblib import load, dump


def assign_row(_output, i):
    print("[Worker %d] Assign for row %d is %d" % (os.getpid(), i, i))
    _output[i] = i
    # print(_output[i])

if __name__ == "__main__":
    folder = tempfile.mkdtemp()
    samples_folder = os.path.join(folder, 'samples')
    assign_folder = os.path.join(folder, 'assign')

    try:
        assign = np.memmap(assign_folder, dtype=np.float64,
                           shape=(10, 8), mode='w+')
        print("Array with shared memory:")
        print(assign)

        # Fork the worker processes to perform computation concurrently
        Parallel(n_jobs=4)(delayed(assign_row)(assign, i)
                           for i in range(5))

        print("Array with new values:")
        print(assign)

    finally:
        try:
            shutil.rmtree(folder)
        except:
            print("Failed to delete: " + folder)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
