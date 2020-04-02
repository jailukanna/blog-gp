---
title: PIL example image pixelate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image pixelate'


Modules used in program: 
* `import PIL.Image as Image`
* `import PIL`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python image pixelate

Python PIL example: image pixelate

```python
#!/usr/bin/env python3

import matplotlib.pyplot as plt
import numpy as np
import PIL
import PIL.Image as Image


plt.axis('off')

dname, iname, fmt = "/Users/Photos/", "avatar", "jpg"

img = Image.open(dname + iname + "." + fmt)
scl = 8

shape = np.array(img).shape

output = img.resize((shape[0]//scl, shape[1]//scl), resample=PIL.Image.LANCZOS)
output = output.resize(shape[:2], resample=PIL.Image.BOX)

plt.imshow(output)
plt.show()

output.save(dname + iname + "-pixelated.png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
