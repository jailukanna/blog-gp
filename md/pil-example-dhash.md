---
title: pil example dhash (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dhash'


Modules used in program: 
* `import hashlib`

## python dhash

Python pil example: dhash

```python
import hashlib
from PIL import Image
#http://www.pythonware.com/products/pil/
#http://blog.iconfinder.com/detecting-duplicate-images-using-python/
#http://en.wikipedia.org/wiki/Hamming_distance

print(hashlib.md5('The quick brown fox jumps over the lazy dog').hexdigest())

image_file1 = open('cat_grumpy_orig.png').read()
print(hashlib.md5(image_file1).hexdigest())

test_image = Image.open('cat_grumpy_modif.png')

print('Image Mode: %s' % test_image.mode)
print('Width: %s px, Height: %s px' % (test_image.size[0], test_image.size[1]))
width, height = test_image.size
pixels = list(test_image.getdata())

hash_size = 8

image = test_image.convert('L').resize(
    (hash_size + 1, hash_size),
    Image.ANTIALIAS,
)
width, height = image.size
pixels = list(image.getdata())

for col in xrange(width):
	print(pixels[col:col+width])

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

print(''.join(hex_string))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
