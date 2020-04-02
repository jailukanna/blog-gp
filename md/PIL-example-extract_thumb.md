---
title: PIL example extract thumb (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'extract thumb'

Functions in program: 
* `def read_tag(endian, fp):`
* `def read_ifd(endian, fp):`
* `def read_tiff(fp):`

Modules used in program: 
* `import struct`
* `import io`

## python extract thumb

Python PIL example: extract thumb

```python
import io
import struct
from fractions import Fraction
from picamera import PiCamera
from PIL import Image
from time import sleep

def read_tiff(fp):
    # The file should contain IFD0, IFD1, and an Exif IFD. As it happens, the
    # Exif IFD also points to interop IFD. We don't actually need the Exif IFD
    # or the interop IFD for extracting the thumbnail but what the hell, let's
    # parse *everything*!
    #
    # NOTE This could probably be faster with mmap but this is a simple demo

    # Figure out the endianness of the TIFF
    endian = {
        b'II': '<',
        b'MM': '>',
        }[fp.read(2)]
    # Check the version number (always 42)
    assert struct.unpack(endian + 'h', fp.read(2)) == (42,)
    # Grab the offset of IFD0; its contents will tell us where IFD1 and the
    # Exif IFD are.
    ifds = {}
    offset, = struct.unpack(endian + 'L', fp.read(4))
    assert offset != 0
    fp.seek(offset)
    ifds['ifd0'], offset = read_ifd(endian, fp)
    fp.seek(offset)
    ifds['ifd1'], offset = read_ifd(endian, fp)
    assert offset == 0
    fp.seek(ifds['ifd0'][34665]) # Exif IFD pointer
    ifds['exif'], offset = read_ifd(endian, fp)
    assert offset == 0
    fp.seek(ifds['exif'][40965]) # Interoperability IFD pointer
    ifds['interop'], offset = read_ifd(endian, fp)
    assert offset == 0
    return ifds

def read_ifd(endian, fp):
    # Read the number of tags in the IFD
    count, = struct.unpack(endian + 'H', fp.read(2))
    tags = {}
    assert count >= 0
    # Loop over reading each tag; last_tag is just to check the tags are all
    # ascending order as the spec demands (you can cut out all these asserts
    # if you want ;)
    last_tag = -1
    while count:
        tag, value = read_tag(endian, fp)
        assert tag >= last_tag
        last_tag = tag
        if value is not None:
            tags[tag] = value
        count -= 1
    # Read and return offset of next IFD
    return (tags,) + struct.unpack(endian + 'L', fp.read(4))

def read_tag(endian, fp):
    # Read and parse a tag. The following table's from the TIFF 6.0 standard
    # and just defines the various data-types, their Python struct-module
    # equivalent spec, and how to unpack the result
    TiffTypes = {
        1:  ('B',  lambda v: v[0]),         # byte
        2:  ('s',  lambda v: v[0].decode('ascii')[:-1]), # ascii (strip NUL terminator)
        3:  ('H',  lambda v: v[0]),         # short
        4:  ('L',  lambda v: v[0]),         # long
        5:  ('LL', lambda v: Fraction(*v)), # rational
        6:  ('b',  lambda v: v[0]),         # signed byte
        7:  ('s',  lambda v: v[0]),         # undefined
        8:  ('h',  lambda v: v[0]),         # signed short
        9:  ('l',  lambda v: v[0]),         # signed long
        10: ('ll', lambda v: Fraction(*v)), # signed rational
        11: ('f',  lambda v: v[0]),         # float
        12: ('d',  lambda v: v[0]),         # double
        }
    TiffTag = struct.Struct(endian + 'HHLL')
    tag, datatype, count, offset = TiffTag.unpack(fp.read(TiffTag.size))
    try:
        typestr, converter = TiffTypes[datatype]
    except KeyError:
        return tag, None
    else:
        valuelen = struct.calcsize(typestr) * count
        typestr = '%s%d%s' % (endian, count, typestr)
        if valuelen <= 4:
            value = struct.unpack(typestr, struct.pack(endian + 'L', offset)[:valuelen])
        else:
            pos = fp.tell()
            try:
                fp.seek(offset)
                value = struct.unpack(typestr, fp.read(valuelen))
            finally:
                fp.seek(pos)
        return tag, converter(value)


# Open a file for the capture, one for the extracted thumbnail, and another for
# the exif data (any of these could be in-memory streams; it's just a little
# easier to debug stuff with actual files)
image_file = io.open('image.jpg', 'w+b')
thumb_file = io.open('thumb.jpg', 'w+b')
exif_file = io.open('exif.tiff', 'w+b')

# Capture an image containing a thumbnail in the Exif data
camera = PiCamera(resolution='720p')
sleep(2)
camera.capture(image_file, quality=95, thumbnail=(320, 180, 50))

# At this point, image.jpg contains the full image, and all the Exif data
# including the thumbnail. Unfortunately, while PIL knows how to extract the
# Exif block from the JPEG (which saves me writing any JPEG parsing code), it
# doesn't know how to extract the thumbnail from the TIFF in the Exif. But
# TIFF's IFD isn't that hard to parse with a little help from struct...
image_file.seek(0)
image = Image.open(image_file)

# Make sure the Exif magic is in place and strip it off, then write the rest
# to the exif_file (which is named ".tiff" because it's actually a TIFF file)
data = image.info['exif']
assert data[:6] == b'Exif\x00\x00'
exif_file.write(data[6:])

# Now use the data in IFD1 to find the thumbnail data
exif_file.seek(0)
ifds = read_tiff(exif_file)
exif_file.seek(ifds['ifd1'][513])
thumb_file.write(exif_file.read(ifds['ifd1'][514]))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
