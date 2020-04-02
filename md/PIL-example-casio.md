---
title: PIL example casio (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'casio'

Functions in program: 
* `def print_label(dev, img, size=6.0, gray=3, cut_mode=CUT_AUTO, nr=1):`
* `def reset(dev):`
* `def cut(dev):`
* `def feed(dev):`
* `def send_label(dev, img, nr=1):`
* `def set_config2(dev, cut_mode=CUT_AUTO):`
* `def set_config1(dev, tape_size, gray_level=3):`
* `def set_label_size(dev, label_size):`
* `def get_tape_size(dev):`
* `def probe(dev):`
* `def ping(dev):`
* `def label(msg, size=(512, 128), font=None):`
* `def loadfont(fontpath=TTFONT, size=128):`
* `def casio2img(cbm):`
* `def img2casio(img):`
* `def maketbl(fn, src=bytes(range(256))):`
* `def bitrev(u8):`
* `def send(dev, data, size=1):`
* `def rr(dev, size=1):`
* `def ww(dev, data):`
* `def cmd(buf, size=0):`
* `def dec(capdata):`
* `def find(vid=VID, pid=PID):`

Modules used in program: 
* `import logging`
* `import PIL.ImageFont`
* `import PIL.ImageDraw`
* `import PIL`
* `import usb`

## python casio

Python PIL example: casio

```python
#!/usr/bin/env python3
# -*- coding: utf-8-unix -*-
"""
Label printing tool for Casio "NameLand" USB label printer.

This module provides control interface to series of label printers
from Casio, namely KLD-300. It probably works with other older
models, but I have only tested this with KLD-300.

KLD-300 is an USB device with vendor-specific class, and uses
EP1_OUT to send command/data and EP2_IN for receiving response/status.

There are 2 types of command: 1-byte command and multi-byte command.
1-byte command is used for basic operation, like feeding and cutting
a tape. Multi-byte command is used for more complex control, like
configuring printer or sending label data. It seems to use
first 4 bytes to express command type, and rest for payload to send data.

Most command only expects 1-byte response code from a printer.
Response code 0x06 seems to be ACK, and 0x15 seems to be NAK.
Other codes are also seen, and generally seem to indicate other
non-ACK states like "busy".

Structure of command/control bytes are reverse engineered to
some extent, but it is not complete. In this script, many bytes are
simply replayed as captured. Comment "ALWAYSSAME" means captured
byte never changed regardless of tape or print(configuration.)

USB packet size is configured as 64bytes, so maximum payload is 60bytes.
This means label data (bitmap data) is split and sent in 60-byte block.

Label image format is a simple 1-bit (black and white) bitmap with
bits packed into a byte. Bytes are in column-oriented format,
meaning bytes in column 1 is sent before any bytes in column 2 is sent.
Also, bit order in each byte is in little endian order.

Label image is expected to be either in 128xN or 64xN bitmap.
Larger tape requires 128xN bitmap, and smaller tape requires 64xN bitmap.

To ease printing of usual "WIDTHx128" or "WIDTHx64" bitmap,
img2casio() and casio2img() functions may be used to convert
standard PIL image into Casio-styled transposed/bitflipped PIL image.

"""

import usb

import PIL
import PIL.ImageDraw
import PIL.ImageFont

import logging

log = logging.getLogger(__name__)

######################################################################

VID = 0x07cf
PID = 0x4106

EP_OUT = 0x01 | usb.ENDPOINT_OUT
EP_IN  = 0x02 | usb.ENDPOINT_IN

ACK = b"\x06"
NAK = b"\x15"

CUT_FULL = 0x00
CUT_AUTO = 0x01
CUT_NONE = 0xFF

TTFONT = '/usr/share/fonts/truetype/fonts-japanese-gothic.ttf'
#TTFONT = '/usr/share/fonts/truetype/mikachan/mikachan.ttf'

######################################################################

def find(vid=VID, pid=PID):
    """Find Casio label printer"""
    for bus in usb.busses():
        for dev in bus.devices:
            if dev.idVendor != vid: continue
            yield dev

def dec(capdata):
    """Decode wireshark-decoded hex dump into bytearray"""
    return bytearray.fromhex(capdata.replace(":", " "))

def cmd(buf, size=0):
    """Return command bytes in 16B buffer"""
    return buf if size <= len(buf) else buf.ljust(size, b"\x00")

def ww(dev, data):
    """Write data to device"""
    return dev.bulkWrite(EP_OUT, data, timeout=10000)

def rr(dev, size=1):
    """Read data from device"""
    return bytes(dev.bulkRead(EP_IN, size, timeout=10000))

def send(dev, data, size=1):
    """Write to and then read from device"""
    ww(dev, data)
    return rr(dev, size=size)

######################################################################

def bitrev(u8):
    """Reverse bit-order of a byte"""
    ret = 0
    for i in range(8):
        if u8 & (1 << i): ret |= (0x80 >> i)
    return ret

def maketbl(fn, src=bytes(range(256))):
    """Create byte translation table"""
    dst = bytes([fn(i) for i in src])
    return bytearray.maketrans(src, dst)

# bit-reversing translation table
RTBL = maketbl(bitrev)

def img2casio(img):
    """Convert PIL image to Casio-style PIL bitmap"""
    tmp = img.convert('1').transpose(PIL.Image.TRANSPOSE)
    buf = tmp.tobytes().translate(RTBL)
    cbm = PIL.Image.frombytes(mode='1', data=buf, size=tmp.size)
    return cbm

def casio2img(cbm):
    """Convert Casio-style PIL bitmap into standard PIL bitmap"""
    buf = cbm.tobytes().translate(RTBL)
    img = PIL.Image.frombytes(mode='1', data=buf, size=cbm.size)
    return img.transpose(PIL.Image.TRANSPOSE)

def loadfont(fontpath=TTFONT, size=128):
    """Return PIL ImageFont loaded from given fontfile"""
    return PIL.ImageFont.truetype(fontpath, size)

def label(msg, size=(512, 128), font=None):
    """Create a label in standard PIL bitmap (not in Casio-style)"""
    img = PIL.Image.new('1', size)

    draw = PIL.ImageDraw.Draw(img)
    draw.font = font if font else PIL.ImageFont.load_default()

    msgsize = draw.font.getsize(msg)
    ydelta = img.size[1] - msgsize[1]

    draw.text((0, ydelta >> 1), msg, 1)
    return img

######################################################################

def ping(dev):
    """Send first command in the capture. Should return ACK"""
    data = dec("02:02:04:00:00:04:03:01:00:00:00:00:00:00:00:00")
    return send(dev, data)

def probe(dev):
    """Send second command in the capture. Returns 6B of data"""
    data = dec("02:1d:00:00:00:00:00:00:00:00:00:00:00:00:00:00")
    return send(dev, data, 6)

def get_tape_size(dev):
    """Check tape size"""
    data = dec("02:1a:00:00:00:00:00:00:00:00:00:00:00:00:00:00")
    return send(dev, data, 6)

def set_label_size(dev, label_size):
    """Check if label and tape size matches"""
    data = dec("02:17:02:00:81:00:00:00:00:00:00:00:00:00:00:00")
    if label_size == 6.0:
        data[4:5] = b"\x81\x00"
    elif label_size == 9.0:
        data[4:5] = b"\x85\x00"
    elif label_size == 18.0:
        data[4:5] = b"\x87\x03"
    else:
        return NAK
    return send(dev, data)

def set_config1(dev, tape_size, gray_level=3):
    """Configure tape size (3.5-24.0) and gray level (1-5)"""
    data = dec("02:09:06:00:01:00:ff:ff:00:00:00:00:00:00:00:00")

    if tape_size in (6.0, 9.0):
        data[4] = 0x01
    else:
        data[4] = 0x00

    # gray level: 1:FE, 2:FF, 3:00, 4:01, 5:02
    data[8] = 0xFF & (gray_level - 3)

    return send(dev, data)

def set_config2(dev, cut_mode=CUT_AUTO):
    """Configure tape cut mode"""
    data = dec("02:19:01:00:01:00:00:00:00:00:00:00:00:00:00:00")
    data[4] = cut_mode
    return send(dev, data)

def send_label(dev, img, nr=1):
    """Send label image in PIL bitmap with 64xN or 128xN size"""

    # data must be split into payload size
    USB_PAYLOAD_SIZE = 60

    # prepare data to send. use dummy data if not given
    if img is None:
        buf = bytearray(USB_PAYLOAD_SIZE)
    else:
        buf = bytearray(img.tobytes())

        # align by USB payload size
        padlen = USB_PAYLOAD_SIZE - len(buf) % USB_PAYLOAD_SIZE
        buf.extend([0] * padlen)
    buflen = len(buf)

    for i in range(nr):
        # send each block
        for i in range(0, buflen - USB_PAYLOAD_SIZE, USB_PAYLOAD_SIZE):
            ret = send(dev, b"\x02\xfe\x3c\x00" + buf[i:i+USB_PAYLOAD_SIZE])

        # send last block?
        i = buflen - USB_PAYLOAD_SIZE
        #ret = send(dev, b"\x02\xfe\x30\x00" + buf[i:i+USB_PAYLOAD_SIZE])
        ret = send(dev, b"\x02\xfe\x0c\x00" + buf[i:i+USB_PAYLOAD_SIZE])

        # mark end-of-label?
        data = dec("02:04:00:00:00:00:00:00:00:00:00:00:00:00:00:00")
        ret = send(dev, data)

        # move tape a little?
        ret = send(dev, b"\x0c")

    return ret

def feed(dev):
    """Feed some tape. Should return ACK."""
    data = dec("02:1b:01:00:15:00:00:00:00:00:00:00:00:00:00:00")
    return send(dev, data)

def cut(dev):
    """Cut tape. Should return ACK. Can be called anytime"""
    return send(dev, b"\x07")

def reset(dev):
    """Reset device. No return value. May feed some tape"""
    ww(dev, b"\x18")

######################################################################

def print_label(dev, img, size=6.0, gray=3, cut_mode=CUT_AUTO, nr=1):
    """Print label by replaying what was captured"""

    ret = ping(dev)
    ret = probe(dev)

    # ALWAYSSAME
    data = dec("02:82:00:00:00:00:00:00:00:00:00:00:00:00:00:00")
    ret = send(dev, data, 5)

    # size check
    ret = get_tape_size(dev)
    ret = set_label_size(dev, size)

    # ALWAYSSAME
    ret = send(dev, dec("02:01:00:00:00:00:00:00:00:00:00:00:00:00:00:00"))
    ret = send(dev, dec("02:1c:01:00:01:00:00:00:00:00:00:00:00:00:00:00"))
    ret = send(dev, dec("02:0d:01:00:40:00:00:00:00:00:00:00:00:00:00:00"))

    # configure
    ret = set_config1(dev, tape_size=size, gray_level=gray)
    ret = set_config2(dev, cut_mode=cut_mode)

    # print
    ret = send_label(dev, img, nr)

    ret = feed(dev)
    ret = cut(dev)

    reset(dev)

if __name__ == '__main__' and '__file__' in globals():
    dev = None
    try:
        dev = next(find()).open()
        dev.claimInterface(0)

        ttfont = loadfont()
        img = label("ABC", size=(512, 128), font=ttfont)
        cbm = img2casio(img)
        print_label(dev, cbm.resize((64, 256)), size=6.0)
    finally:
        if dev: dev.releaseInterface()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
