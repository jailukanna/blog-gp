---
title: pil example png2jpg (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'png2jpg'

Functions in program: 
* `def main():`

Modules used in program: 
* `import sys`

## python png2jpg

Python pil example: png2jpg

```python
#!/usr/bin/env python
# http://stackoverflow.com/questions/9166400/convert-rgba-png-to-rgb-with-pil
# http://stackoverflow.com/questions/1962795/how-to-get-alpha-value-of-a-png-image-with-pil
import sys
from PIL import Image

def main():
    png = Image.open(sys.argv[1])
    png.load()
    background = Image.new("RGB", png.size, (255, 255, 255))
    try:
        background.paste(png, mask=png.split()[3])
    except IndexError:
        background.paste(png, mask=png.convert('RGBA').split()[3])

    background.save(sys.argv[2], 'JPEG', quality=90)

if __name__ == "__main__":
        main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
