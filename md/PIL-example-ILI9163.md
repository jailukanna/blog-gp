---
title: PIL example ILI9163 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ILI9163'

Functions in program: 
* `def rgb(r, g, b): return ((b>>3) << 11) | ((g>>2) << 5) | (r>>3)`

Modules used in program: 
* `import Adafruit_GPIO.SPI as SPI`
* `import Adafruit_GPIO as GPIO`
* `import time`

## python ILI9163

Python PIL example: ILI9163

```python
import time

try:
    import numpy
    def image_to_data(image):
        pb = numpy.array(image.convert('RGB')).astype('uint16')
        color = ((pb[:,:,2] >> 3) << 11) | ((pb[:,:,1] >> 2) << 5) | (pb[:,:,0] >> 3)
        return numpy.dstack(((color >> 8) & 0xFF, color & 0xFF)).flatten().tolist()
except ImportError:
    def image_to_data(image):
        """Generator function to convert a PIL image to 16-bit 565 RGB bytes."""
        pixels = image.convert('RGB').load()
        width, height = image.size
        for y in range(height):
            for x in range(width):
                r,g,b = pixels[(x,y)]
                color = rgb(r, g, b)
                yield (color >> 8) & 0xFF
                yield color & 0xFF

from PIL import Image
from PIL import ImageDraw

import Adafruit_GPIO as GPIO
import Adafruit_GPIO.SPI as SPI

# define size
ILI9163_HEIGHT                         = 128
ILI9163_WIDTH                         = 128

# ILI9163 LCD Controller Commands
ILI9163_CMD_NOP                     = 0x00
ILI9163_CMD_SOFT_RESET              = 0x01
ILI9163_CMD_GET_RED_CHANNEL         = 0x06
ILI9163_CMD_GET_GREEN_CHANNEL       = 0x07
ILI9163_CMD_GET_BLUE_CHANNEL        = 0x08
ILI9163_CMD_GET_PIXEL_FORMAT        = 0x0C
ILI9163_CMD_GET_POWER_MODE          = 0x0A
ILI9163_CMD_GET_ADDRESS_MODE        = 0x0B
ILI9163_CMD_GET_DISPLAY_MODE        = 0x0D
ILI9163_CMD_GET_SIGNAL_MODE         = 0x0E
ILI9163_CMD_GET_DIAGNOSTIC_RESULT   = 0x0F
ILI9163_CMD_ENTER_SLEEP_MODE        = 0x10
ILI9163_CMD_EXIT_SLEEP_MODE         = 0x11
ILI9163_CMD_ENTER_PARTIAL_MODE      = 0x12
ILI9163_CMD_ENTER_NORMAL_MODE       = 0x13
ILI9163_CMD_EXIT_INVERT_MODE        = 0x20
ILI9163_CMD_ENTER_INVERT_MODE       = 0x21
ILI9163_CMD_SET_GAMMA_CURVE         = 0x26
ILI9163_CMD_SET_DISPLAY_OFF         = 0x28
ILI9163_CMD_SET_DISPLAY_ON          = 0x29
ILI9163_CMD_SET_COLUMN_ADDRESS      = 0x2A
ILI9163_CMD_SET_PAGE_ADDRESS        = 0x2B
ILI9163_CMD_WRITE_MEMORY_START      = 0x2C
ILI9163_CMD_WRITE_LUT               = 0x2D
ILI9163_CMD_READ_MEMORY_START       = 0x2E
ILI9163_CMD_SET_PARTIAL_AREA        = 0x30
ILI9163_CMD_SET_SCROLL_AREA         = 0x33
ILI9163_CMD_SET_TEAR_OFF            = 0x34
ILI9163_CMD_SET_TEAR_ON             = 0x35
ILI9163_CMD_SET_ADDRESS_MODE        = 0x36
ILI9163_CMD_SET_SCROLL_START        = 0x37
ILI9163_CMD_EXIT_IDLE_MODE          = 0x38
ILI9163_CMD_ENTER_IDLE_MODE         = 0x39
ILI9163_CMD_SET_PIXEL_FORMAT        = 0x3A
ILI9163_CMD_WRITE_MEMORY_CONTINUE   = 0x3C
ILI9163_CMD_READ_MEMORY_CONTINUE    = 0x3E
ILI9163_CMD_SET_TEAR_SCANLINE       = 0x44
ILI9163_CMD_GET_SCANLINE            = 0x45
ILI9163_CMD_READ_ID1                = 0xDA
ILI9163_CMD_READ_ID2                = 0xDB
ILI9163_CMD_READ_ID3                = 0xDC
ILI9163_CMD_FRAME_RATE_CONTROL1     = 0xB1
ILI9163_CMD_FRAME_RATE_CONTROL2     = 0xB2
ILI9163_CMD_FRAME_RATE_CONTROL3     = 0xB3
ILI9163_CMD_DISPLAY_INVERSION       = 0xB4
ILI9163_CMD_SOURCE_DRIVER_DIRECTION = 0xB7
ILI9163_CMD_GATE_DRIVER_DIRECTION   = 0xB8
ILI9163_CMD_POWER_CONTROL1          = 0xC0
ILI9163_CMD_POWER_CONTROL2          = 0xC1
ILI9163_CMD_POWER_CONTROL3          = 0xC2
ILI9163_CMD_POWER_CONTROL4          = 0xC3
ILI9163_CMD_POWER_CONTROL5          = 0xC4
ILI9163_CMD_VCOM_CONTROL1           = 0xC5
ILI9163_CMD_VCOM_CONTROL2           = 0xC6
ILI9163_CMD_VCOM_OFFSET_CONTROL     = 0xC7
ILI9163_CMD_WRITE_ID4_VALUE         = 0xD3
ILI9163_CMD_NV_MEMORY_FUNCTION1     = 0xD7
ILI9163_CMD_NV_MEMORY_FUNCTION2     = 0xDE
ILI9163_CMD_POSITIVE_GAMMA_CORRECT  = 0xE0
ILI9163_CMD_NEGATIVE_GAMMA_CORRECT  = 0xE1
ILI9163_CMD_GAM_R_SEL               = 0xF2

ROT0 = 0        # portrait
ROT90 = 96      # landscape
ROT180 = 160    # flipped portrait
ROT270 = 192    # flipped landscape

def rgb(r, g, b): return ((b>>3) << 11) | ((g>>2) << 5) | (r>>3)

class ILI9163(object):
    # Initialise the display with the require screen orientation
    def __init__(self, gpio, spi, a0, rst, rotation = ROT0):
        self.width = ILI9163_WIDTH
        self.height = ILI9163_HEIGHT

        self.gpio = gpio
        self.spi = spi
        self.a0 = a0
        self.rst = rst

        self.spi.set_mode(0)
        self.spi.set_bit_order(SPI.MSBFIRST)
        self.spi.set_clock_hz(64000000)
        # Create an image buffer.
        self.buffer = Image.new('RGB', (self.width, self.height))

        self.gpio.setup(self.a0, GPIO.OUT)
        self.gpio.setup(self.rst, GPIO.OUT)

        self.gpio.output(self.rst, GPIO.HIGH)

        # Hardware reset the LCD
        self.reset()

        self.writeCommand(ILI9163_CMD_EXIT_SLEEP_MODE)
        time.sleep(0.005) # Wait for the screen to wake up

        self.writeCommand(ILI9163_CMD_SET_PIXEL_FORMAT)
        self.writeData(0x05) # 16 bits per pixel

        self.writeCommand(ILI9163_CMD_SET_GAMMA_CURVE)
        self.writeData(0x04) # Select gamma curve 3

        self.writeCommand(ILI9163_CMD_GAM_R_SEL)
        self.writeData(0x01) # Gamma adjustment enabled

        self.writeCommand(ILI9163_CMD_POSITIVE_GAMMA_CORRECT)
        self.writeData([0x3f, 0x25, 0x1c, 0x1e, 0x20, 0x12, 0x2a, 0x90, 0x24, 0x11, 0x00, 0x00, 0x00, 0x00, 0x00])

        self.writeCommand(ILI9163_CMD_NEGATIVE_GAMMA_CORRECT)
        self.writeData([0x20, 0x20, 0x20, 0x20, 0x05, 0x00, 0x15, 0xa7, 0x3d, 0x18, 0x25, 0x2a, 0x2b, 0x2b, 0x3a])

        self.writeCommand(ILI9163_CMD_FRAME_RATE_CONTROL1)
        self.writeData([0x08, 0x08]) # DIVA = 8, VPA = 8

        self.writeCommand(ILI9163_CMD_DISPLAY_INVERSION)
        self.writeData(0x07) # NLA = 1, NLB = 1, NLC = 1 (all on Frame Inversion)

        self.writeCommand(ILI9163_CMD_POWER_CONTROL1)
        self.writeData([0x0a, 0x02]) # VRH = 10:  GVDD = 4.30, VC = 2: VCI1 = 2.65

        self.writeCommand(ILI9163_CMD_POWER_CONTROL2)
        self.writeData(0x02) # BT = 2: AVDD = 2xVCI1, VCL = -1xVCI1, VGH = 5xVCI1, VGL = -2xVCI1

        self.writeCommand(ILI9163_CMD_VCOM_CONTROL1)
        self.writeData([0x50, 0x5b]) # VMH = 80: VCOMH voltage = 4.5, VML = 91: VCOML voltage = -0.225

        self.writeCommand(ILI9163_CMD_VCOM_OFFSET_CONTROL)
        self.writeData(0x40) # nVM = 0, VMF = 64: VCOMH output = VMH, VCOML output = VML

        self.writeCommand(ILI9163_CMD_SET_COLUMN_ADDRESS)
        self.writeData([0x00, 0x00, 0x00, 0x7f]) # XSH, XSL, XEH, XEL (128 pixels x)

        self.writeCommand(ILI9163_CMD_SET_PAGE_ADDRESS)
        self.writeData([0x00, 0x00, 0x00, 0x7f]) # 128 pixels y

        # Select display orientation
        self.writeCommand(ILI9163_CMD_SET_ADDRESS_MODE)
        #self.writeData(rotation)
        self.setRotation(rotation)

        # Set the display to on
        self.writeCommand(ILI9163_CMD_SET_DISPLAY_ON)
        self.writeCommand(ILI9163_CMD_WRITE_MEMORY_START)

    def writeCommand(self, address):
        # set D/C pin for command
        self.gpio.output(self.a0, GPIO.LOW)
        # send data by SPI
        self.spi.write([address])

    def writeData(self, data):
        # set D/C pin for data
        self.gpio.output(self.a0, GPIO.HIGH)
        data = [data] if isinstance(data, int) else data
        # send data by SPI
        self.spi.write(data)

    def writeData16(self, word):
        # set D/C pin for data
        self.gpio.output(self.a0, GPIO.HIGH)
        if isinstance(word, int):
            data = [word]
        else:
            data = []
            for w in word:
                data.append((w >> 8) & 0xFF)
                data.append(w & 0xFF)
        # send data by SPI
        self.spi.write(data)

    #set coordinate for print(or other function)
    def setAddress(self, x1 = 0, y1 = 0, x2 = ILI9163_WIDTH - 1, y2 = ILI9163_HEIGHT - 1):
        yoff = 0
        xoff = 0
        if self.rotation == ROT0: yoff = 32
        elif self.rotation == ROT90: xoff = 32

        self.writeCommand(ILI9163_CMD_SET_COLUMN_ADDRESS)
        self.writeData16([x1 + xoff, x2 + xoff])

        self.writeCommand(ILI9163_CMD_SET_PAGE_ADDRESS)
        self.writeData16([y1 + yoff, y2 + yoff])
        # memory write
        self.writeCommand(ILI9163_CMD_WRITE_MEMORY_START)

    # Reset the LCD hardware
    def reset(self):
        # Reset pin is active low (0 = reset, 1 = ready)
        self.gpio.output(self.rst, GPIO.LOW)
        time.sleep(0.05)

        self.gpio.output(self.rst, GPIO.HIGH)
        time.sleep(0.120)

    def setRotation(self, rotation):
        self.rotation = rotation
        self.writeCommand(ILI9163_CMD_SET_ADDRESS_MODE)
        self.writeData(rotation)

    def display(self, image=None):
        """Write the display buffer or provided image to the hardware.  If no
        image parameter is provided the display buffer will be written to the
        hardware.  If an image is provided, it should be RGB format and the
        same dimensions as the display hardware.
        """
        # By default write the internal buffer to the display.
        if image is None:
            image = self.buffer
        # Set address bounds to entire display.
        self.setAddress()
        # Convert image to array of 16bit 565 RGB data bytes.
        # Unfortunate that this copy has to occur, but the SPI byte writing
        # function needs to take an array of bytes and PIL doesn't natively
        # store images in 16-bit 565 RGB format.
        pixelbytes = list(image_to_data(image))
        # Write data to hardware.
        self.writeData(pixelbytes)

    def clear(self, color=(0,0,0)):
        """Clear the image buffer to the specified RGB color (default black)."""
        width, height = self.buffer.size
        self.buffer.putdata([color]*(width*height))

    def draw(self):
        """Return a PIL ImageDraw instance for 2D drawing on the image buffer."""
        return ImageDraw.Draw(self.buffer)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
