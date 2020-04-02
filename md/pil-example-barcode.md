---
title: pil example barcode (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'barcode'


Modules used in program: 
* `import glob`
* `import os`

## python barcode

Python pil example: barcode

```python
#!/usr/bin/env python

# Takes a bunch of screenshots from a movie and converts them into a movie barcode.

# You need to capture the screenshots with the command line version of VLC, which should in OS X,
# Windows and Linux (I think). The command (in a terminal) to do this is: 
# vlc /path/to/movie/file --video-filter=scene --scene-prefix=movie --scene-ratio=90 --scene-path=/path/to/folder/to/store/images/
# Note that it plays the movie in real time, so it takes a while.

# You might need to create an alias to vlc with: alias vlc='/Applications/VLC.app/Contents/MacOS/VLC'

# You'll probably also want to calculate a scene-ratio specific for the movie that you're capturing
# screenshots from, so that you don't waste space or have too few screenshots. The scene-ratio tells 
# VLC how often to capture an image e.g. every 90 frames for the example above. Assume that there are
# 24 frames per second. Take the length of the movie in minutes, multiply by 60, multiply by 24, then
# divide by 1280, and use this number for the scene-ratio.

# NOTE. You need to have the Python Image Library (PIL) installed for this program to work.
# And this program should be in the same folder as your captured screenshots.

# When you've done all that, run the program by typing 'python barcode.py', sit back, and wait for
# your beautiful movie barcode.

# glob import for listing the thumbnails.
# PIL for processing the thumbnails, and creating the barcode.

import os
import glob
from PIL import Image

# Creates a list of the thumbnails.

thumbs = glob.glob('movie*.png')
i = 1

# Loops through the thumbnails and resizes them into 1 pixel wide slices.

for thumb in thumbs:
	try:
		print('converting thumb', str(i), 'of', str(len(thumbs)))
		im = Image.open(thumb)
		h, w = im.size
		slice = im.resize((1, h))
		slice.save('slice' + str(i) + '.png')
		i += 1
	except IOError:
		print('could not open', thumb)
		
# Creates a list of the slices we just made.

slices = glob.glob('slice*.png')
j = 0

# Calculates width and height of image. Creates a new image to paste the slices into.

w = len(slices)
h = int(w/2.667)
barcode = Image.new('RGB', (w, h))

# Loops through slices. Converts to single colour. Pastes into image from left to right.

for slice in slices:
	print('pasting slice', str(j + 1), 'of', str(w))
	im = Image.open(slice)
	pixel = im.resize((1, 1))
	newslice = pixel.resize((1, h))
	barcode.paste(newslice, (j, 0))
	j += 1

# Resizes and saves the image.

barcode = barcode.resize((1280, 480))
barcode.save('barcode.jpg', 'JPEG')

# Cleans up and removes all of the files (apart from the barcode!)
# Comment out this next section if you want to keep the images for later.

print('cleaning up')

for thumb in thumbs:
	os.remove(thumb)
	
for slice in slices:
	os.remove(slice)

print('processing complete')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
