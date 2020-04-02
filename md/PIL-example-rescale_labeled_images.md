---
title: PIL example rescale labeled images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'rescale labeled images'

Functions in program: 
* `def main(argv):`
* `def parseArgs(args):`
* `def rescale_labeled_images(path, destination, size):`
* `def create_dir(directory):`

Modules used in program: 
* `import xml.etree.ElementTree as ET`
* `import sys`
* `import glob`
* `import os`

## python rescale labeled images

Python PIL example: rescale labeled images

```python
import os
import glob
import sys
import xml.etree.ElementTree as ET
from PIL import Image

# Get the Current Dir
root = os.getcwd()

def create_dir(directory):
	'''
	Create a new dir, if not already exists
	'''
	if not os.path.exists(directory):
		os.makedirs(directory)

def rescale_labeled_images(path, destination, size):
	
	global root
	
	# Create the destination directory
	create_dir(os.path.join(root, destination))
	
	# Get the destination size
	dest_width, dest_height = size
	
	for xml_file in glob.glob(path + '/*.xml'):
		tree = ET.parse(xml_file)
		root = tree.getroot()
		
		# Get the image file first
		image_name = root.find('filename').text
		image_path = os.path.join(path, image_name)
	
		# Resize Image
		pil_image = Image.open(open(image_path, 'rb'))
		pil_image_resize = pil_image.resize(size)
		pil_image_resize.save(os.path.join(destination, image_name))
		
		width, height = pil_image.size
		
		width_scale = dest_width / width
		height_scale = dest_height / height
		
		# Update XML
		#################
		
		# Change the size part
		size_part = root.find('size')
		for child in size_part:
			if child.tag == 'width':
				child.text = str(dest_width)
			elif child.tag == 'height':
				child.text = str(dest_height)
		
		# Update the Box for each object
		for member in root.findall('object'):
			for child in member:
				if child.tag == 'bndbox':
					for point in child:
						value = int(point.text)
						if point.tag[0] == 'x':
							value = int(value * width_scale)
						else:
							value = int(value * height_scale)
						point.text = str(value)
		
		# Build the new file name
		xml_file_name = xml_file.replace(path, '')
		xml_file_name = xml_file_name.replace('\\', '')
		tree.write(os.path.join(destination, xml_file_name))

def parseArgs(args):
	
	# Possible arguments
	input_dir = None
	output_dir = None
	img_size = None
	
	i = 1
	while  i < len(args):
		if args[i] == '-i':
			input_dir = args[i+1]
			i += 2
		elif args[i] == '-o':
			output_dir = args[i+1]
			i += 2
		elif args[i] == '-s':
			img_size = (int(args[i+1]),int(args[i+2]))
			i += 3
			
	return input_dir, output_dir, img_size
		
def main(argv):
    input_dir, output_dir, img_size = parseArgs(argv)
    rescale_labeled_images(input_dir, output_dir, img_size)
    pass

if __name__ == "__main__":
    main(sys.argv)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
