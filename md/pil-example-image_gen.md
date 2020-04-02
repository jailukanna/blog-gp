---
title: pil example image gen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image gen'

Functions in program: 
* `def create_sample_pics():`
* `def generate_base_imgs():`

Modules used in program: 
* `import random`
* `import glob`
* `import os`
* `import PIL.Image`

## python image gen

Python pil example: image gen

```python
import PIL.Image
import os
import glob
import random

if not os.path.exists("out"):
    os.mkdir("out")

if not os.path.exists("backgrounds_resized"):
    os.mkdir("backgrounds_resized")

ball_masks = glob.glob("ball_masks/*")
bg_pics = glob.glob("backgrounds/*")

OUTPUT_DIMENSIONS = (320, 240)
print("Generating...")


def generate_base_imgs():
    for num, img_path in enumerate(bg_pics):

        # Make some baselines if they don't already exist
        new_img = PIL.Image.new("RGB", OUTPUT_DIMENSIONS)
        bg = PIL.Image.open(img_path)
        bg = bg.resize(OUTPUT_DIMENSIONS, PIL.Image.ANTIALIAS)
        new_img.paste(bg)
        new_img.save("backgronuds_resized/neg_{}.png".format(num))

# Resize the base pics so we have negative

def create_sample_pics():
    for i in range(5):

        new_img = PIL.Image.new("RGB", OUTPUT_DIMENSIONS)
        bg = PIL.Image.open(random.choice(bg_pics))
        ball = PIL.Image.open(random.choice(ball_masks))

        # Resize and insert the background image

        bg = bg.resize(OUTPUT_DIMENSIONS, PIL.Image.ANTIALIAS)

        new_img.paste(bg)

        # Resize the ball img
        # We want some randomness for loc, size (within bounds), and whether or not it even includes a ball.

        aspect_ratio = min(new_img.width / ball.width, new_img.height / ball.height)

        resize_ratio = random.randint(2, 8)

        ball = ball.resize((int((ball.width * aspect_ratio) // resize_ratio),
                            int((ball.height * aspect_ratio) // resize_ratio)),
                           PIL.Image.ANTIALIAS)

        # Get loc

        # Generate some coords
        # It might be partly out of the frame but that's fine
        new_coords = (random.randint(0, OUTPUT_DIMENSIONS[0] - (OUTPUT_DIMENSIONS[0] // 8)),
                      random.randint(0, OUTPUT_DIMENSIONS[1] - (OUTPUT_DIMENSIONS[1] // 8)))

        new_img.paste(ball, new_coords, ball)

        new_img.save("out/posi_{}.png".format(i), format="PNG")


if __name__ == "__main__":
    create_sample_pics()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
