---
title: PIL example test hid drive tool (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test hid drive tool'

Functions in program: 
* `def test_draw_drive_info_to_lcd():`
* `def test_get_drive_firmware_version():`

Modules used in program: 
* `import stat`
* `import os`
* `import subprocess`
* `import logging`

## python test hid drive tool

Python PIL example: test hid drive tool

```python
import logging
import subprocess
import os
import stat

logging.basicConfig(level=logging.DEBUG)

log = logging.getLogger("TestHidDriveTool")

"""
def test_get_drive_firmware_version():
    print("\n")
    cwd = os.getcwd()
    filename = os.path.join(cwd, "flash_gordon/bin/HidDriveTool")
    print("Executable file {}".format(filename))

    # here i will change the 'HidDriveTool' file permissions to only execute the file by owner (pi)
    os.chmod(filename, stat.S_IEXEC)

    # subprocess that returns the stdout to info when called
    info = subprocess.Popen([filename, '--identify'],
                            stdout=subprocess.PIPE).communicate()[0]

    # decoding the info and splitting the string to list
    info = info.decode("utf-8").split()

    # Probs not the best way to handle this string and list stuff but works for poc me thinks
    matching_drive_info = [s for s in info[0].split(';') if "D" in s][0][2:]
    print("Current drive: {}".format(matching_drive_info))

    firmware_version = info[1]
    print("Current drive FW version: {}".format(firmware_version))

    drive_info = [matching_drive_info, firmware_version]
    return drive_info
"""


def test_draw_drive_info_to_lcd():

    #drive_info = test_get_drive_firmware_version()

    #drive_name = drive_info[0]
    #drive_fw = drive_info[1]


    import Adafruit_SSD1306

    from PIL import Image
    from PIL import ImageDraw
    from PIL import ImageFont

    #####################
    ## HARD CODED DATA ##
    #####################
    versionOld = 08.12
    versionNew = 88.88

    # Raspberry Pi pin configuration:
    RST = 24

    disp = Adafruit_SSD1306.SSD1306_128_64(rst=RST)
    disp.begin()

    disp.clear()
    disp.display()

    width = disp.width
    height = disp.height

    cwd = os.getcwd()

    confirm_image = os.path.join(cwd, "flash_gordon/assets/confirm.ppm")

    fontSmall = ImageFont.truetype(os.path.join(cwd, "flash_gordon/assets/fonts/04B_03__.TTF"), 8)
    fontMedium = ImageFont.truetype(os.path.join(cwd, 'flash_gordon/assets/fonts/8-Bit Madness.ttf'), 16)
    fontLarge = ImageFont.truetype(os.path.join(cwd, 'flash_gordon/assets/fonts/LeviWindows.ttf'), 22)
    fontHuge = ImageFont.truetype(os.path.join(cwd, 'flash_gordon/assets/fonts/8-Bit Madness.ttf'), 32)

    def showIntro():
        image = Image.open("flash_gordon/assets/connect.ppm").convert('1')
        draw = ImageDraw.Draw(image)
        disp.image(image)
        disp.display()

    def showConfirm(versionOld, versionNew):
        image = Image.open(confirm_image).convert('1')
        draw = ImageDraw.Draw(image)
        draw.text((91, -9), 'v.'+str(versionOld), font=fontLarge, fill=255)
        draw.text((0, 40), str(versionNew), font=fontHuge, fill=255)
        disp.image(image)
        disp.display()

    def showProgress():
        image = Image.open("flash_gordon/assets/progress.ppm").convert('1')
        draw = ImageDraw.Draw(image)
        disp.image(image)
        disp.display()

    def showDone(versionNew):
        image = Image.open("flash_gordon/assets/done.ppm").convert('1')
        draw = ImageDraw.Draw(image)
        draw.text((31, 51), 'v.'+str(versionNew), font=fontSmall, fill=255)
        disp.image(image)
        disp.display()

    #showIntro()
    showConfirm(versionOld, versionNew)
    #showProgress()
    #showDone(versionNew)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
