---
title: PIL example trimmer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'trimmer'


Modules used in program: 
* `import os`
* `import math`
* `import os`
* `import argparse`
* `import cv2`

## python trimmer

Python PIL example: trimmer

```python
# inputs:
# input file
# newpath
# filename
# frames every x seconds

import cv2
import argparse
import os
import math
from PIL import Image
import os
from os import listdir
from os.path import isfile, join

parser = argparse.ArgumentParser(description="Convert video to screenshots")
parser.add_argument('input_path', type=str,
                    help='Input video')
parser.add_argument('output_path', type=str,
                    help='Output file directory')
parser.add_argument('numSeconds', type=int,
                    help='# of frames to skip')


args = parser.parse_args()

try:
    # Create target Directory
    os.mkdir(args.output_path)
except FileExistsError:
	pass

path = os.path.abspath(args.input_path)
files_list = [join(path, f) for f in listdir(path) if isfile(join(path, f))]

output_dir = os.path.abspath(args.output_path)

for file in files_list:
	vidcap = cv2.VideoCapture(file)
	framerate = vidcap.get(cv2.CAP_PROP_FPS)
	total_frames = int(vidcap.get(cv2.CAP_PROP_FRAME_COUNT))

	skip_num = int(framerate*args.numSeconds)
	total_screenshots = int(total_frames / skip_num)
	numDigits = len(str(total_screenshots))

	v_width = vidcap.get(cv2.CAP_PROP_FRAME_WIDTH)   # float
	v_height = vidcap.get(cv2.CAP_PROP_FRAME_HEIGHT)

	#Make Blank Image with space for 4 Columns and X (screenshots / 4) rows
	numCols = math.ceil(total_screenshots / 4)

	single_width = 200
	single_height = math.ceil(v_height*200/v_width)

	img_width = single_width*4 + 10*3
	img_height = single_height*numCols + (numCols-1)*10

	dimensions = (img_width, img_height)
	main_image = Image.new("RGB", dimensions)


	success,image = vidcap.read()
	count = 0
	success = True
	printCount = 0

	name = os.path.split(file)
	name = name[1]

	name = os.path.splitext(name)
	name = name[0]

	print(name)

	while success:
	    success,image = vidcap.read()
	    if count > 0 and count%skip_num == 0 :
	    	path = args.output_path
	    	if path[-1] == "/":
	    		path = path[:-1]
	    	img = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
	    	im_pil = Image.fromarray(img)
	    	im_pil.thumbnail((single_width, single_height), Image.ANTIALIAS)

	    	col = printCount % 4
	    	row = printCount // 4

	    	x_offset = col*single_width + 10*(col)
	    	y_offset = single_height*row + 10*(row)
	    	main_image.paste(im_pil, (x_offset, y_offset))

	    	# cv2.imwrite("%s/%s_%s.jpg" % (args.output_path, name, str(printCount).zfill(numDigits)), image)
	    	printCount += 1
	    	print("Created screenshot #%d" % printCount)
	    count+=1
	print(total_screenshots)
	main_image.show()
	filename = "%s/%s_all.jpg" % (output_dir, name)
	main_image.save(filename)
	print("Saved %s" % filename)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
