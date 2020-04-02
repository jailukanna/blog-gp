---
title: PIL example screencap website (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screencap website'


Modules used in program: 
* `import StringIO`

## python screencap website

Python PIL example: screencap website

```python
"""
This is an example of how to take a screen capture using Selenium and
PIL. The code uses the PhantomJS driver so be sure to install
per the requirements section below.

Note that Selenium can save a screen capture directly to disk but the
code here saves it as a StringIO object so we can demonstrate how to
get the data into PIL.  From there we can edit it, store in a database,
save out to s3, etc. without needing to create a file on disk

Requirements:
  * pip install selenium pillow
  * install needed OS libraries:
        Mac: $ brew install libtiff libjpeg webp little-cms2 phantomjs

        Linux: sudo apt-get install python-dev libtiff5-dev libjpeg8-dev zlib1g-dev \
               libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev \
               python-tk phantomjs
               
Notes:
 * The code below will cause Selenium to throw TimeoutException if the page 
   doesnt load after 20 seconds.  If you wish to handle you can find it and 
   other errors in selenium.common.exceptions

"""

##########################################################################
## Imports
##########################################################################

import StringIO

from selenium import webdriver
from PIL import Image


##########################################################################
## Classes
##########################################################################

class ScreenCapExample(object):

    WINDOW_SIZE = [1024, 768]
    SELENIUM_TIMEOUT = 20

    def process(self, address):
        # screencap and return a PIL image object
        img = self._screen_capture(address)

        # fake operation for demonstration
        modified_img = self._do_stuff_to_img(img)

        # save the image to disk
        img.save('%s.png' % address, 'PNG')


    def _do_stuff_to_img(self, img):
        """
        Placeholder method to modify the image and return a copy
        """
        new_img = img.copy
        return new_img


    def _screen_capture(self, address):
        """
        Performs a screen capture for a website homepage and then stores
        the image as a PIL Image object
        """
        url = 'http://' + address

        driver = webdriver.PhantomJS()
        driver.set_page_load_timeout(self.SELENIUM_TIMEOUT)
        driver.set_window_size(*self.WINDOW_SIZE)

        try:
            driver.get(url)
            png_data =  driver.get_screenshot_as_png()
            img = Image.open(StringIO.StringIO(png_data))
        finally:
            driver.quit()

        return img

if __name__ == '__main__':
    example = ScreenCapExample()
    example.process('google.com')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
