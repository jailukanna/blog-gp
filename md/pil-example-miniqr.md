---
title: pil example miniqr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'miniqr'

Functions in program: 
* `def decode(file_path):`
* `def encode(content, output, scale):`

## python miniqr

Python pil example: miniqr

```python
#!/bin/env python3


def encode(content, output, scale):
    import pyqrcode
    qr = pyqrcode.create(content)
    qr.png(output, scale)


def decode(file_path):
    import PIL
    import zbarlight

    with open(file_path, 'rb') as image_file:
        image = PIL.Image.open(image_file)
        image.load()

    for code in zbarlight.scan_codes('qrcode', image):
        if isinstance(code, bytes):
            print(code.decode())
        else:
            print(code)


if __name__ == '__main__':
    import argparse

    parser = argparse.ArgumentParser(description='''
A trivial QR encoder and decoder.''', epilog='''
Encoding requires the pyqrcode and pypng modules.
Decoding requires the PIL and zbarlight modules.
The zbarlight module requires the zbar or zbar-headless system package.

https://pypi.python.org/pypi/PyQRCode/
https://pypi.python.org/pypi/zbarlight/
http://zbar.sourceforge.net/
''', formatter_class=argparse.RawDescriptionHelpFormatter)
    pgroup = parser.add_mutually_exclusive_group()
    pgroup.add_argument('-e', '--encode', help='encode content to QR image',
                        action='store_true')
    pgroup.add_argument('-d', '--decode', action='store_true', default=True,
                        help='decode a QR image to content (default)')
    parser.add_argument('-o', '--out', default='qr.png',
                        help='name of output png file (default: %(default)s)')
    parser.add_argument('-s', '--scale', type=int, default=8,
                        help='scale of encoded file (default: %(default)d)')
    parser.add_argument('file_path_or_content',
                        help='file to be decoded or content to be encoded')
    args = parser.parse_args()

    if args.encode:
        encode(args.file_path_or_content, args.out, args.scale)
    elif args.decode:
        decode(args.file_path_or_content)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
