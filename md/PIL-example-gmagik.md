---
title: PIL example gmagik (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gmagik'

Functions in program: 
* `def magik(im: PIL.Image.Image, out: RawIOBase):`
* `def gmagik(gif: PIL.Image.Image, out: RawIOBase):`

Modules used in program: 
* `import wand.image`
* `import PIL.Image`

## python gmagik

Python PIL example: gmagik

```python
#!/usr/bin/env python3

import PIL.Image
from PIL import ImageSequence
import wand.image
from io import BytesIO, RawIOBase


def gmagik(gif: PIL.Image.Image, out: RawIOBase):
    total = gif.n_frames
    out_frames = []
    for idx, frame in enumerate(ImageSequence.Iterator(gif), start=1):
        print(f"Processing frame {idx}/{total}")
        b = BytesIO()
        magik(frame, b)
        out_frames.append(PIL.Image.open(b))
    print(f"Done processing {total} frames")
    out_frames[0].save(out, "GIF", loop=0, save_all=True, append_images=out_frames[1:])
    out.seek(0)


def magik(im: PIL.Image.Image, out: RawIOBase):
    b = BytesIO()
    im.save(b, "gif")
    b.seek(0)
    im = wand.image.Image(file=b)
    im.transform(resize="800x800>")
    im.liquid_rescale(
        width=int(im.width * 0.5), height=int(im.height * 0.5), delta_x=1, rigidity=0
    )
    im.liquid_rescale(
        width=int(im.width * 1.5), height=int(im.height * 1.5), delta_x=2, rigidity=0
    )
    im.resize(im.width, im.height)
    im.save(file=out)
    out.seek(0)


if __name__ == "__main__":
    import argparse, sys

    parser = argparse.ArgumentParser("gmagik")
    subparsers = parser.add_subparsers(required=True)

    for action in ('magik', 'gmagik'):
        action_parser = subparsers.add_parser(action)
        action_parser.add_argument("input", nargs=1)
        action_parser.add_argument("output", nargs=1)
        action_parser.set_defaults(func=globals()[action])

    args = parser.parse_args()
    inp = args.input[0]
    out = args.output[0]
    try:
        inp = PIL.Image.open(inp).copy()
        with open(out, "wb") as f:
            args.func(inp, f)
    except Exception as e:
        print("an exception occurred:", file=sys.stderr)
        raise


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
