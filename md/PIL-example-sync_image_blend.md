---
title: PIL example sync image blend (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'sync image blend'


Modules used in program: 
* `import PIL`
* `import os, glob`
* `import re, random`

## python sync image blend

Python PIL example: sync image blend

```python
"""
@Author: Md Mahedi Hasan
"""
# python modules
import re, random
import os, glob
from skimage import io, util
from scipy import ndarray, ndimage
from PIL import Image
import PIL


# directory settting
local_dir = os.path.dirname(os.path.abspath(__file__))

source_dir = os.path.join(local_dir, "dhaka_for_synthetic_images")
icon_dir = os.path.join(local_dir, "gazipur_icon")
output_dir = os.path.join(local_dir, "sync_gz")

os.makedirs(output_dir, exist_ok = True)

# working on source direcotory
os.chdir(source_dir)
src_image_list = glob.glob( "*.jpg")
src_text_list = glob.glob("*.txt")

# working on icon directory
os.chdir(icon_dir)
icon_image_list = glob.glob( "*.jpg")

# sorting
src_image_list = sorted(src_image_list, key = lambda x : int(x.split(".")[0][3:]))
src_text_list = sorted(src_text_list, key = lambda x : int(x.split(".")[0][3:]))
icon_image_list = sorted(icon_image_list, key = lambda x : int(x.split(".")[0]))

print(src_image_list)
print(src_text_list)
print(icon_image_list)

for num, text_file in enumerate(src_text_list):
    with open(os.path.join(source_dir, text_file), "r") as file:
        for line in file:
            if(line.split(" ")[0] == "20"):
                print(src_image_list[num])

                # loading same image file of text
                img = Image.open(os.path.join(source_dir, src_image_list[num]))
                img_w, img_h = img.size  #w,h

                # getting coordinates
                rel_cor = line.split(" ")

                rel_center_x = float(rel_cor[1])
                rel_center_y = float(rel_cor[2])
                rel_img_w = float(rel_cor[3])
                rel_img_h = float(rel_cor[4])

                print(rel_center_x, " ", rel_center_y, " ", rel_img_w, " ", rel_img_h)

                # getting absolute co-ordinates x,y, w,h
                abs_x = (rel_center_x - rel_img_w / 2) *  img_w
                abs_y = ((rel_center_y - rel_img_h / 2) * img_h) + 2
                
                abs_width = rel_img_w * img_w
                abs_height = (rel_img_h * img_h) - 4

                # pick a random icon image and resize it
                rand_number = random.randint(1, len(icon_image_list) - 1)
                icon_img = Image.open(os.path.join(icon_dir, icon_image_list[rand_number - 1]))
                icon_img_res = icon_img.resize((int(abs_width), int(abs_height)), 
                                                resample = PIL.Image.BICUBIC)


                # making bounding box coordinates in imagemagick format (WxH+X+Y)
                #crop = str(abs_width) + "x" + str(abs_height) + "+" + str(abs_x) + "+" + str(abs_y)

                input_path = os.path.join(source_dir, src_image_list[num])
                output_path = os.path.join(output_dir, src_image_list[num])

                img.paste(icon_img_res, (int(abs_x), int(abs_y)))
                io.imsave(output_path, img)
                # os.system("convert -crop " + crop + " " + input_path + " " + output_path)
                
                break


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
