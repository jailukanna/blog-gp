---
title: pil example generate images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'generate images'

Functions in program: 
* `def main():`

Modules used in program: 
* `import datetime`
* `import random`
* `import argparse`
* `import sys`

## python generate images

Python pil example: generate images

```python
from itertools import cycle
import sys
import argparse
import random

from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import datetime

try:
    import pyexiv2
except ImportError:
    pyexiv2 = None


default_font = "/usr/share/fonts/dejavu/DejaVuSans.ttf"


def main():
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument('--prefix', action='store', default="image")
    parser.add_argument('--format', action='append', choices=['jpg', 'png', 'gif', 'webp'])
    parser.add_argument('--image_width', type=int, help='Image width', default=512)
    parser.add_argument('--image_height', type=int, help='Image height', default=512)
    parser.add_argument('--count', type=int, help='Number of files', default=1)
    parser.add_argument('--font', dest='font', action='store', default="")
    parser.add_argument('--exif', dest='exif', action='store_true', default=False)

    args = parser.parse_args()

    if args.exif and pyexiv2 is None:
        sys.stderr.write("pyexiv2 not installed\n")
        sys.exit(1)

    now = datetime.datetime.utcnow()
    now_string = now.strftime("%Y%m%d-%H%M%S")

    if args.font:
        font = ImageFont.truetype(args.font, 16)
    else:
        try:
            font = ImageFont.truetype(default_font, 16)
        except IOError:
            font = ImageFont.load_default()

    image_formats = cycle(args.format or ['jpg'])

    for i in range(args.count):
        id = "%s-%s" % (now_string, i)
        filename = "%s.%s.%s" % (args.prefix, id, next(image_formats))

        bg_color = (random.randint(0, 50), random.randint(0, 50), random.randint(0, 50))
        fg_color = (random.randint(100, 255), random.randint(100, 255), random.randint(100, 255))

        img = Image.new("RGBA", (args.image_width, args.image_height), bg_color)
        draw = ImageDraw.Draw(img)
        draw.text((5, 5), id, fg_color, font=font)
        img.save(filename)

        if args.exif:
            source_image = pyexiv2.ImageMetadata(filename)
            source_image.read()

            source_image['Exif.Image.Make'] = "N/A"
            source_image['Exif.Image.Model'] = "N/A"
            source_image['Exif.Image.DocumentName'] = "Generated image #" + str(i)
            source_image['Exif.Image.ImageDescription'] = "Image " + filename
            source_image['Exif.Image.Software'] = "python %s" % str(sys.version_info)
            source_image['Exif.Photo.UserComment'] = "Comment #" + str(i)

            source_image['Iptc.Application2.Headline'] = ["Generated image #" + str(i)]
            source_image['Iptc.Application2.Caption'] = ["Image " + filename]
            source_image['Iptc.Application2.Keywords'] = ["generated"]

            source_image.write()

        print(filename)


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
