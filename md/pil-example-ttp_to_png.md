---
title: pil example ttp to png (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ttp to png'


Modules used in program: 
* `import sys, io, PIL, PIL.Image, PIL.ImageFile`

## python ttp to png

Python pil example: ttp to png

```python
import sys, io, PIL, PIL.Image, PIL.ImageFile
from PIL import Image
from struct import unpack, pack

'''
https://gbatemp.net/threads/open-a-ttp-ttpf-image-vc-edit.442031/

https://pillow.readthedocs.io/en/4.3.x/reference/Image.html?highlight=rgba#PIL.Image.frombytes
https://pillow.readthedocs.io/en/4.3.x/handbook/writing-your-own-file-decoder.html#file-decoders
https://pillow.readthedocs.io/en/stable/handbook/writing-your-own-file-decoder.html
https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.register_decoder
https://pillow.readthedocs.io/en/latest/reference/ImageFile.html
https://pillow.readthedocs.io/en/4.3.x/handbook/concepts.html#concept-modes
https://pillow.readthedocs.io/en/3.4.x/handbook/image-file-formats.html

https://stackoverflow.com/questions/3397157/how-to-read-a-raw-image-using-pil
https://en.wikipedia.org/wiki/Endianness
https://docs.python.org/3/library/struct.html
https://stackoverflow.com/questions/7983684/how-to-switch-byte-order-of-binary-data
https://stackoverflow.com/questions/846038/convert-a-python-int-into-a-big-endian-string-of-bytes
https://stackoverflow.com/questions/1934624/item-assignment-on-bytes-object-in-python
https://imgur.com/a/5SEEo
'''

class Test(PIL.ImageFile.PyDecoder):
  def decode(self, buffer):
    print(self.mode)
    buffer = bytearray(buffer)
    i = 0
    while i < len(buffer) or (i > 0 and i+4 < len(buffer)):
      buffer[i:i+4] = pack('>I', unpack('<I', buffer[i:i+4])[0])
      i = i+4
    self.set_as_raw(bytes(buffer))
    print(i, len(buffer)-i-1)
    return (len(buffer)-i-1, 0)

PIL.Image.register_decoder('Test', Test)

with open('./face_cgb.ttp', 'rb') as f:
  f.seek(0x1c)
  size = unpack('<HH', f.read(4))
  
  img = Image.frombytes('RGBA', size, f.read(), 'Test')
  
with open('test.png', 'wb') as out:
  img.save(out)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
