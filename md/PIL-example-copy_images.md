---
title: PIL example copy images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'copy images'

Functions in program: 
* `def main():`
* `def dhash_file(filename, hash_size=8):`
* `def dhash(image, hash_size=8):`

Modules used in program: 
* `import shutil`
* `import PIL.Image`
* `import os`
* `import hashlib`
* `import errno`
* `import cPickle`
* `import argparse`

## python copy images

Python PIL example: copy images

```python
# This script creates an image hash database and can then be used
# to copy image files to a directory, but only when they are not
# already in that directory (by the image hash).

from __future__ import print_function
from itertools import islice, izip
from six.moves import range
import argparse
import cPickle
import errno
import hashlib
import os
import PIL.Image
import shutil


def dhash(image, hash_size=8):
  """ Compute a hash from a PIL *image*.
  Thanks to http://blog.iconfinder.com/detecting-duplicate-images-using-python/ . """

  # Grayscale and shrink the image in one step.
  image = image.convert('L').resize(
    (hash_size + 1, hash_size),
    PIL.Image.ANTIALIAS,
  )

  pixels = list(image.getdata())

  # Compare adjacent pixels.
  difference = []
  for row in xrange(hash_size):
    for col in xrange(hash_size):
      pixel_left = image.getpixel((col, row))
      pixel_right = image.getpixel((col + 1, row))
      difference.append(pixel_left > pixel_right)

  # Convert the binary array to a hexadecimal string.
  decimal_value = 0
  hex_string = []
  for index, value in enumerate(difference):
    if value:
      decimal_value += 2**(index % 8)
    if (index % 8) == 7:
      hex_string.append(hex(decimal_value)[2:].rjust(2, '0'))
      decimal_value = 0

  return ''.join(hex_string)


def dhash_file(filename, hash_size=8):
  return dhash(PIL.Image.open(filename))


def main():
  parser = argparse.ArgumentParser()
  parser.add_argument('dest_dir')
  parser.add_argument('--hash-size', type=int, default=8)
  parser.add_argument('--dry-copy', action='store_true')
  parser.add_argument('--update-db', action='store_true')
  parser.add_argument('--db-file', default='_image_hash_db')
  parser.add_argument('--copy-from')
  parser.add_argument('--override-dest')
  args = parser.parse_args()

  if args.update_db:
    print("Updating Image Hash Database...")
    database = {}
    for fn in os.listdir(args.dest_dir):
      fnfull = os.path.join(args.dest_dir, fn)
      try:
        database[dhash_file(fnfull, args.hash_size)] = fn
      except IOError:
        pass
    with open(args.db_file, 'wb') as fp:
      cPickle.dump(database, fp)
  else:
    if not os.path.isfile(args.db_file):
      print("error: Image Hash Database does not exist, use --update-db")
      return errno.ENOENT
    with open(args.db_file, 'rb') as fp:
      database = cPickle.load(fp)
    assert isinstance(database, dict)

  if args.copy_from:
    for fn in os.listdir(args.copy_from):
      fnfull = os.path.join(args.copy_from, fn)
      try:
        hash_value = dhash_file(fnfull, args.hash_size)
      except IOError:
        continue
      if hash_value in database:
        print("{0}: image already exists: {1}".format(fn, database[hash_value]))
      else:
        print("{0}: copying ...".format(fn))
        if not args.dry_copy:
          if args.override_dest:
            dest_base = os.path.join(args.override_dest, fn)
          else:
            dest_base = os.path.join(args.dest_dir, fn)

          # Make sure the destination filename does not exist.
          index = 0
          dest = dest_base
          while os.path.isfile(dest):
            print("  warning: {0} already exists".format(os.path.basename(dest)))
            dest = dest_base + '_{:0>5}'.format(index)
            index += 1

          shutil.copyfile(fnfull, dest)


if __name__ == '__main__':
  main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
