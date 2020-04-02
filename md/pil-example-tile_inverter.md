---
title: pil example tile inverter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'tile inverter'

Functions in program: 
* `def process_folder(day_folder_name, night_folder_name, relative_path=None, max_count=None):`
* `def invert_image(input_file, output_path):`

Modules used in program: 
* `import sys`
* `import os`

## python tile inverter

Python pil example: tile inverter

```python
import os
import sys

# from PIL import Image
# import PIL.ImageOps


def invert_image(input_file, output_path):
    # image = Image.open(input_file).convert('RGB')
    # PIL.ImageOps.invert(image).save(output_path)
    os.system("magick %s -color-matrix '6x3: -1 0 0 0 0 1 0 -1 0 0 0 1 0 0 -1 0 0 1' %s" % (input_file, output_path))


def process_folder(day_folder_name, night_folder_name, relative_path=None, max_count=None):
    count = 0

    if relative_path is None:
        location = day_folder_name
    else:
        location = "%s/%s" % (relative_path, day_folder_name)

    for folder, _sub_folders, files in os.walk(location):
        # Make sure we're only processing the images
        images = [f for f in files if f.endswith('.png')]

        for image in images:
            # Get the path to the output
            output_folder = folder.replace(day_folder_name, night_folder_name)

            # Create it if it doesn't already exist
            if not os.path.exists(output_folder):
                os.makedirs(output_folder)

            # Give the full input and output paths to the invert image function
            invert_image("%s/%s" % (folder, image), "%s/%s" % (output_folder, image))
            count = count + 1
            if count % 10 == 0:
                # Print some progress every 5 images
                sys.stdout.write('.')
                sys.stdout.flush()

            if max_count is not None and count >= max_count:
                return


relative_path = None
max_count = None
# If there is a relative location, it'll be the first passed-in argument
if (len(sys.argv) > 1):
    relative_path = sys.argv[1]

    if (len(sys.argv) > 2):
        max_count = int(sys.argv[2])

day_folder = 'base'
night_folder = 'night'

process_folder(day_folder, night_folder, relative_path, max_count)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
