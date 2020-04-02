---
title: PIL example resize benchmark (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'resize benchmark'

Functions in program: 
* `def benchmark_f(trials, f, target, method=None):`
* `def ski_resize(x, target, method):`
* `def pil_resize(x, target, method=Image.NEAREST):`

Modules used in program: 
* `import numpy as np`
* `import time`

## python resize benchmark

Python PIL example: resize benchmark

```python
import time
import numpy as np

from skimage.transform import resize
from PIL import Image


def pil_resize(x, target, method=Image.NEAREST):
    img = Image.fromarray(x)
    return np.array(img.resize(target, resample=method))


def ski_resize(x, target, method):
    return np.uint8(resize(x / 255, target) * 255)


def benchmark_f(trials, f, target, method=None):
    elapsed = 0
    img_size = (160, 120)  # Size of Atari frame

    for i in range(trials):
        x = np.random.randint(0, 255, img_size, dtype=np.uint8)
        start = time.time()
        _ = f(x, target, method)  # noqa
        elapsed += time.time() - start
    print("%s-%s: %d trials  |  total=%.2fs  |  mean=%.2fs." %
          (f.__name__, 'n' if method is None else method, trials, elapsed,
              elapsed / trials))


if __name__ == "__main__":
    trials = 1000
    target = (84, 84)
    benchmark_f(trials, pil_resize, target, Image.NEAREST)
    benchmark_f(trials, pil_resize, target, Image.LANCZOS)
    benchmark_f(trials, pil_resize, target, Image.BILINEAR)
    benchmark_f(trials, pil_resize, target, Image.BICUBIC)
    benchmark_f(trials, pil_resize, target, Image.BOX)
    benchmark_f(trials, pil_resize, target, Image.HAMMING)
    benchmark_f(trials, ski_resize, target)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
