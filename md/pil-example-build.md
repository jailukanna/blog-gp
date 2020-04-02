---
title: pil example build (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'build'

Functions in program: 
* `def load_image(filename):`
* `def pil_to_tc(pilImage):`

Modules used in program: 
* `import os`
* `import glob`
* `import progressbar`
* `import turicreate as tc`
* `import the necessary packages`

## python build

Python pil example: build

```python
import the necessary packages
from PIL import Image
import turicreate as tc
import progressbar
import glob
import os

def pil_to_tc(pilImage):
	# grab the PIL image dimensions, convert the data into a byte
	# array, and then build the turicreate image
	(W, H) = pilImage.size[:2]
	data = bytearray([b for a in pilImage.getdata() for b in a])
	image = tc.Image(_image_data=data, _width=W, _height=H,
		_channels=3, _format_enum=2, _image_data_size=len(data))
  
	# return the turicreate image
	return image

def load_image(filename):
  pilImage = Image.open(os.path.sep.join([INPUT_PATH,filename]))
  image = pil_to_tc(pilImage)
  return image

# initialize the input datasets
INPUT_PATH = "../vehicles"

# Load all the CSV files to a single SFrame
data = tc.SFrame.read_csv(os.path.sep.join([INPUT_PATH, "*.csv"]))

# Create the desired annotations in the format turicreate expects them - object's
# center coordinates plus height and width
data['x'] = (data['startX'] + data['endX']) / 2
data['y'] = (data['startY'] + data['endY']) / 2
data['width'] = data['endX'] - data['startX']
data['height'] = data['endY'] - data['startY']

# Pack the annotations into a dictionary
data = data.pack_columns(['x', 'y', 'width', 'height'], dtype=dict, new_column_name="coordinates")
data['type'] = 'rectangle'
data = data.pack_columns(['coordinates', 'type', 'label'], dtype=dict, new_column_name='annotations')

# Group all the annotation by the image path
final_data = data.groupby('imagePath', {'annotations': tc.aggregate.CONCAT('annotations')})

# Load the images themselves from the path
final_data['image'] = final_data['imagePath'].apply(load_image)
final_data = final_data.remove_column('imagePath')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
