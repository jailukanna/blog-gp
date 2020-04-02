---
title: PIL example shredder (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'shredder'

Functions in program: 
* `def main():`

Modules used in program: 
* `import PIL.Image`
* `import random`
* `import argparse`

## python shredder

Python PIL example: shredder

```python
#!/usr/bin/env python
import argparse
import random
import PIL.Image

def main():
    parser = argparse.ArgumentParser(description="Image Unshredder")
    parser.add_argument("-s", "--shred-width", default=32, type=int,
                        help="width of each shred")
    parser.add_argument("-o", "--out-file", default="unshredded.png",
                        help="output PNG image file name")
    parser.add_argument("imagefile", help="original image")
    args = parser.parse_args()
    
    image = PIL.Image.open(args.imagefile)
    width, shred = image.size[0], args.shred_width
    if width % shred != 0:
        print("width of shred and image  are mismatched (%r, %r)" % 
              (shred, width))
        return
    
    count = width // shred
    order = list(range(count))
    for i in range(count - 1):
        j = random.randint(i + 1, count - 1)
        order[i], order[j] = order[j], order[i]
        pass
    print("shuffle as %r" % (order,))
    
    out = PIL.Image.new(image.mode, image.size)
    for i, n in enumerate(order):
        crop = image.crop((shred * n, 0, shred * (n + 1), image.size[1]))
        out.paste(crop, (shred * i, 0))
        pass
    out.save(args.out_file, "PNG")
    return

if __name__ == "__main__":
    main()
    pass



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
