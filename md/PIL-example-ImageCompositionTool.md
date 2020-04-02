---
title: PIL example ImageCompositionTool (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ImageCompositionTool'


Modules used in program: 
* `import PIL`
* `import datetime`
* `import os`

## python ImageCompositionTool

Python PIL example: ImageCompositionTool

```python
import os
import datetime
import PIL
from PIL import Image
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw


class ImageCompositionTool(object):
    """
    """

    OUT_COUNT_TRESHOLD = 3

    def __init__(self, options, captures):
        """
        Keyword arguments:
        options -- dict of options:
            padding - Padding used here and there.
            size - Width and height tuple of the generated images.
            background_color - 3- or 4-tuple color for the background, defaults to black.
            background_color_mode - Defaults to rgb.
            font - Path to font family.
            font_size - Font size.
        captures -- list of dicts that contain capture configurations. A capture configuration contains:
            name - Name of the capture.
            image_folder -  Folder path for the images.
            placeholder_path - Path for the placeholder image.
            size - Size of the source image.
            target_size - Scale the image to this size. Leave empty for no change.
            target_position - Paste the image here on the generated image.
            crop - Crop configuration:
                placeholder_path - Path for the placeholder image.
                size - Size of the crop.
                position - Topleft position of the crop.
                target_size - Scale the crop to this size.
                target_position - Paste the crop here on the generated image.
        """
        self.captures = captures
        self.options = options

        if "background_color" not in self.options:
            self.options["background_color"] = 0

        if "background_color_mode" not in self.options:
            self.options["background_color_mode"] = "RGB"

        self.font = ImageFont.truetype(self.options["font"], self.options["font_size"])
        self.image_files = {}
        for capture in captures:
            capture["out_count"] = 0
            capture["placeholder_image"] = Image.open(capture["placeholder_path"])
            capture["previous"] = capture["placeholder_path"]
            if "crop" in capture:
                capture["crop"]["placeholder_image"] = Image.open(capture["crop"]["placeholder_path"])
            self.add_files(capture["image_folder"], capture["name"])
        self.captures = captures

    def generate_image(self):
        # create a blank image
        image = Image.new(
            self.options["background_color_mode"],
            self.options["size"],
            self.options["background_color"]
        )

        index = 1
        # loop through dict in date order (date is dict key)
        for file_created in sorted(self.image_files):

            entry = self.image_files[file_created]

            for capture in self.captures:
                if capture["name"] not in entry:
                    capture["out_count"] += 1
                    if capture["out_count"] >= self.OUT_COUNT_TRESHOLD:
                        capture["previous"] = capture["placeholder_path"]
                    entry[capture["name"]] = capture["previous"]
                else:
                    capture["out_count"] = 0
                current_capture_path = entry[capture["name"]]
                capture["previous"] = current_capture_path

                capture_image = ""
                crop_image = ""
                if current_capture_path == capture["placeholder_path"]:
                    capture_image = capture["placeholder_image"]
                    if "crop" in capture:
                        crop_image = capture["crop"]["placeholder_image"]
                else:
                    capture_image = Image.open(current_capture_path)
                    if "crop" in capture:
                        crop = capture["crop"]
                        crop_image = capture_image.crop(
                            (
                                crop["position"][0],
                                crop["position"][1],
                                crop["position"][0] + crop["size"][0],
                                crop["position"][1] + crop["size"][1]
                            )
                        )
                        crop_image = self.resize_image(crop_image, crop)
                    capture_image = self.resize_image(capture_image, capture)

                image.paste(
                    capture_image,
                    (
                        capture["target_position"][0],
                        capture["target_position"][1]
                    )
                )

                if "crop" in capture:
                    image.paste(
                        crop_image,
                        (
                            capture["crop"]["target_position"][0],
                            capture["crop"]["target_position"][1]
                        )
                    )

                self.draw_date(image, file_created)

            image.save("result/test {}.png".format(index), optimize=True)
            index += 1

    def resize_image(self, image, capture):
        if "target_size" in capture:
            if capture["target_size"][0] < capture["size"][0]:
                image = image.resize(capture["target_size"], Image.LANCZOS)
            else:
                image = image.resize(capture["target_size"], Image.BICUBIC)
        return image

    def add_files(self, path, name):
        for root, dirs, files in os.walk(path):
            for file in files:
                filepath = os.path.join(root, file)
                # get file created and convert to int (precision: seconds)
                file_created = datetime.datetime.fromtimestamp(int(os.stat(filepath).st_mtime))
                # round to nearest 5
                rounded_seconds = int(5 * round(float(file_created.second)/5))
                # if 60, get next minute
                if rounded_seconds > 59:
                    file_created = file_created + datetime.timedelta(minutes=1)
                    rounded_seconds = 0
                file_created = file_created.replace(second=rounded_seconds)

                # add to dict or modify current dict
                if file_created not in self.image_files:
                    self.image_files[file_created] = {}
                self.image_files[file_created][name] = filepath


    def draw_date(self, image, date):
        draw = ImageDraw.Draw(image)
        # draw a rectangle to 
        draw.rectangle(
            (
                (
                    1280 + 350,
                    150
                ),
                (
                    1280 + 350 + 800,
                    150 + 100
                )
            ),
            fill=(0, 0, 0)
        )
        draw.text((1280 + 350, 150), date.strftime("%Y.%m.%d %H:%M:%S"), font=self.font)

compositer = ImageCompositionTool(
    {
        'padding': 150,
        'size': (2560, 1440),
        'font': 'Rubik-Bold.ttf',
        'font_size': 72,
    },
    [
        {
            'name': 'first',
            'image_folder': 'images_first',
            'placeholder_path': 'placeholder_first.png',
            'size': (2560, 1440),
            'target_size': (1280, 720),
            'target_position': (0, 0),
            'crop': {
                'placeholder_path': 'placeholder_crop.png',
                'size': (320, 240),
                # crop webcam that is on the bottom right, just above taskbar (30px)
                'position': (
                    2560 - 320, # width - crop width
                    1440 - 240 - 30, # height - crop height - taskbar height
                    2560, # width
                    1440 - 30 # height - taskbar height
                ),
                'target_size': (960, 720),
                'target_position': (
                    int(2560 - 960 - 150),
                    int(1440 / 2 - 720 / 2)
                )
            }
        },
        {
            'name': 'second',
            'image_folder': 'images_second',
            'placeholder_path': 'placeholder_second.png',
            'size': (1920, 1080),
            'target_size': (1280, 720),
            'target_position': (0, 720)
        }
    ]
)
compositer.generate_image()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
