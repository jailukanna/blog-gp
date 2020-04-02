---
title: pil example char imggen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'char imggen'

Functions in program: 
* `def __main():`
* `def __init_args():`
* `def generate(charset_info, outdir):`
* `def save_img(img, filepath):`
* `def generate_char_img(char, fontname='Osaka', size=(64, 64)):`

Modules used in program: 
* `import schema`
* `import docopt`
* `import logging`
* `import sys`
* `import os`

## python char imggen

Python pil example: char imggen

```python
#!/usr/bin/env python
# -*- coding: utf-8-unix -*-

"""generate single character image.

Usage:
    char_imggen.py [-i INPUT] [-o OUTDIR] [-v]
    char_imggen.py -h|--help
    char_imggen.py --version

Options:
    -i --input INPUT    input filename [default: ./target_chars.json].
    -o --output OUTDIR  output dirname [default: ./].
    -h --help           show this help message.
    --version           show this script version.
    -v --verbose        logging level [default: False].
"""

import os
import sys
import logging

import docopt
import schema

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

def generate_char_img(char, fontname='Osaka', size=(64, 64)):
    logging.debug('generate ch[{}].'.format(char))

    img=Image.new('L', size, 'white')
    draw = ImageDraw.Draw(img)
    fontsize = int(size[0]*0.8)
    font = ImageFont.truetype(fontname, fontsize)

    # adjust charactor position.
    char_displaysize = font.getsize(char)
    offset = tuple((si-sc)//2 for si, sc in zip(size, char_displaysize))
    assert all(o>=0 for o in offset)

    # adjust offset, half value is right size for height axis.
    draw.text((offset[0], offset[1]//2), char, font=font, fill='#000')
    return img

def save_img(img, filepath):
    logging.debug('save img to {}.'.format(filepath))
    img.save(filepath, 'png')


def generate(charset_info, outdir):
    for fontname, chars in charset_info.items():
        for ch in chars:
            if ch.isupper():
                logging.info('modify filepath for uppercase char[{}] '.format(ch)
                    + 'because of case-insensitive of PIL.Image.save.')
                filepath = os.path.join(
                    outdir, '{}_caps{}_img.png'.format(fontname, ch))
            else:
                filepath = os.path.join(
                    outdir, '{}_{}_img.png'.format(fontname, ch))
            img = generate_char_img(ch, fontname)
            save_img(img, filepath)
    return True

def __init_args():
    args = docopt.docopt(__doc__, version='0.0.1')
    try:
        s = schema.Schema({
            '--input': schema.Use(open), 
            '--output': schema.And(str, os.path.exists), 
            '--help': bool,
            '--version': bool,
            '--verbose': bool
        })
        args = s.validate(args)
    except schema.SchemaError as e:
        sys.stderr.write(e)
        sys.exit(1)

    if args['--verbose']:
        # debug.
        logging.basicConfig(level=logging.DEBUG,
            format='%(asctime)s [%(levelname)s] [%(filename)s:%(lineno)d] %(message)s')
    else:
        # release.
        logging.basicConfig(level=logging.INFO,
            format='%(asctime)s [%(levelname)s] %(message)s')

    logging.debug('{}'.format(args))

    return args

def __main():
    import json
    args = __init_args()
    charset_info = json.load(args['--input'])
    return generate(charset_info, args['--output'])

if __name__ == '__main__':
    if __main():
        sys.exit(0)
    else:
        sys.exit(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
