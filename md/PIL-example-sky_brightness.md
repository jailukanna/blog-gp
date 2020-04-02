---
title: PIL example sky brightness (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'sky brightness'

Functions in program: 
* `def sky_brightness(iso, fr, exps, midhist) :`

Modules used in program: 
* `import PIL.ImageStat`
* `import PIL.ExifTags`
* `import PIL.Image`
* `import argparse`
* `import numpy as np`

## python sky brightness

Python PIL example: sky brightness

```python
#!/usr/bin/env python

# Program to calculate the sky brightness of a JPEG image of the night sky
# Assumptions:
# The EXIF data for the image contains the following valid tags:
#   ISOSpeedRatings, ApertureValue, ExposureTime

import numpy as np
import argparse
import PIL.Image
import PIL.ExifTags
import PIL.ImageStat

# Function to calculate the sky brightness based on iso, aperture, exposure time and mid histogram gray value of image
def sky_brightness(iso, fr, exps, midhist) :
    pct = (midhist/256.0) * 100.0
    exp = exps/60.0                    # Exposure time in mins
    mins = exp * (iso / 800.0)*(50.0 / pct)*(4.0 / fr)*(4.0 / fr)
    skybrightness = 13.93 + (2.5 * np.log10(mins*60.0))

    return skybrightness


# Main program
if __name__ == "__main__":

    ap = argparse.ArgumentParser()
    ap.add_argument("image_file", help="The name of the image_file to analyse")
    args = vars(ap.parse_args())
    image_name = args["image_file"]

    # Open the image and get EXIF data
    img = PIL.Image.open(image_name)
    
    exif = {
        PIL.ExifTags.TAGS[k]: v
        for k, v in img._getexif().items()
        if k in PIL.ExifTags.TAGS
    }

    # print(exif)
    iso = exif.get("ISOSpeedRatings")
    if iso < 50 : iso = 800
    fr = float(exif.get("ApertureValue")[0]) / float(exif.get("ApertureValue")[1])
    exps = float(exif.get("ExposureTime")[0]) / float(exif.get("ExposureTime")[1])

    # Print the image settings
    print
    print("Image:", image_name)
    print("ISO:", iso)
    print("Aperture:", fr)
    print("Exposure time:", exps)
    print


    # Calculate median of grey image
    stat = PIL.ImageStat.Stat(img.convert('L'))
    midhist = stat.median[0]

    print("Median grey histogram value:", midhist)
    print("Sky brightness of image using median is {:0.2f}".format(sky_brightness(iso, fr, exps, midhist)), "mag/arcsec2")
    print


    # Calculate using mode of grey image
    midhist = np.bincount(np.array(img.convert('L')).flatten()).argmax()

    print("Mode grey histogram value:", midhist)
    print("Sky brightness of image using mode is {:0.2f}".format(sky_brightness(iso, fr, exps, midhist)), "mag/arcsec2")
    print

    """
    # Split the image into rgb, and calculate sky brightness for each colour
    r, g, b = img.split()
    midhist = np.bincount(np.array(r).flatten()).argmax()
    print("Mode red histogram value:", midhist)
    print("Sky brightness of image using mode is {:0.2f}".format(sky_brightness(iso, fr, exps, midhist)), "mag/arcsec2")
    print
    midhist = np.bincount(np.array(g).flatten()).argmax()
    print("Mode green histogram value:", midhist)
    print("Sky brightness of image using mode is {:0.2f}".format(sky_brightness(iso, fr, exps, midhist)), "mag/arcsec2")
    print
    midhist = np.bincount(np.array(b).flatten()).argmax()
    print("Mode blue histogram value:", midhist)
    print("Sky brightness of image using mode is {:0.2f}".format(sky_brightness(iso, fr, exps, midhist)), "mag/arcsec2")
    print
    """


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
