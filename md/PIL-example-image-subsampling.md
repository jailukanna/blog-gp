---
title: PIL example image-subsampling (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image-subsampling'

Functions in program: 
* `def get_iterator(dataset, batch_size, image_crop_operation):`
* `def initialize_dataset(number_of_test_cases, height, width, color_channels):`

Modules used in program: 
* `import random`
* `import numbers`
* `import numpy as np`

## python image-subsampling

Python PIL example: image-subsampling

```python
# -*- coding: utf-8 -*-
# @Author: rishabh
# @Date:   2018-06-21 11:08:50
# @Last Modified by:   rishabhthukralprivate
# @Last Modified time: 2020-03-01 20:19:55
# @Description: Definition of a class that can be used for image subsampling.


# Imports for basic python modules
import numpy as np
import numbers
import random

from PIL import Image
# Imports END

# Global Decelerations and definitions
BATCH_SIZE = 32
COLOR_CHANNELS = 3
WIDTH = 1920
HEIGHT = 1080
TEST_DATASET_SIZE = 45
PATCH_SIZE = 512
# Global Decelerations and definitions END


# [START Class to define random crop operation. Object of this class will be
# used to cut random   patches from the given image]
class RandomCrop(object):
	"""
	Crop the given PIL Image at a random location.
	Args:
		- size (sequence or int): Desired output size of the crop. If size
		is an int instead of sequence like (h, w), a square crop (size, size)
		is made.
	"""

	def __init__(self, size):
		if isinstance(size, numbers.Number):
			self.size = (int(size), int(size))
		else:
			self.size = size

	@staticmethod
	def crop(img, yc_patch, xc_patch, target_height, target_width):
		"""
		Crop the given PIL Image.
		Args:
			- img (PIL Image): Image to be cropped.
			- yc_patch: Upper pixel coordinate.
			- xc_patch: Left pixel coordinate.
			- target_height: Height of the cropped image.
			- target_width: Width of the cropped image.
		Returns:
			- PIL Image: Cropped image.
		"""

		return img.crop(
			(
				xc_patch, yc_patch,
				xc_patch + target_width,
				yc_patch + target_height)
		)

	@staticmethod
	def get_params(img, output_size):
		"""
		Get parameters for ``crop`` for a random crop.
		Args:
			- img (PIL Image): Image to be cropped.
			- output_size (tuple): Expected output size of the crop.
		Returns:
			- tuple: params (yc_patch, xc_patch, target_height, target_width)
			to be passed to ``crop`` for random crop.
		"""

		width_img, height_img = img.size
		target_height, target_width = output_size

		if width_img == target_width and height_img == target_height:
			return 0, 0, height_img, width_img

		yc_patch = random.randint(0, height_img - target_height)
		xc_patch = random.randint(0, width_img - target_width)

		return yc_patch, xc_patch, target_height, target_width

	def __call__(self, img):
		"""
		Args:
			- img (PIL Image): Image to be cropped.
		Returns:
			- PIL Image: Cropped image.
		"""

		yc_patch, xc_patch, target_height, target_width = self.get_params(
			img, self.size
		)

		return self.crop(img, yc_patch, xc_patch, target_height, target_width)
# [END]


# [START Function to make dataset using the dataset using random numbers]
def initialize_dataset(number_of_test_cases, height, width, color_channels):
	"""
	Args:
		- number_of_test_cases: Number of objects to be included in the
		dummy dataset.
		- height: Height of images in dummy dataset.
		- width: Width of images in dummy dataset.
		- color_channels: Number of color channels included
	Returns:
		- dataset: Dataset of images with specified parameters
	"""

	dataset = []

	for _ in range(number_of_test_cases):
		dummy_image_data = np.random.randint(
			256, size=[height, width, color_channels])
		dataset.append(dummy_image_data)

	return dataset
# [END]


# [START Function to iterate over dataset and return data in mini-batches]
def get_iterator(dataset, batch_size, image_crop_operation):
	"""
	Args:
		- dataset: Dataset of images from which minibatches to be generated.
		- batch_size: Batch size
		- image_crop_operation: An object of RandomCrop class initialized as
		- such to select patches of given size
	Return:
		- _iterator: Generator object to serve as iterator
	"""

	def _iterator(dataset, batch_size, image_crop_operation):
		count = 0
		batch = []

		while dataset:
			data_obj = dataset.pop()
			data_obj_as_image = Image.fromarray(data_obj, "RGB")

			patch = image_crop_operation(data_obj_as_image)
			patch_as_np_array = np.transpose(np.asarray(patch))

			batch.append(patch_as_np_array)

			count += 1

			if dataset:
				if count == batch_size:
					batch = np.asarray(batch)
					yield batch
					batch = []
			else:
				batch = np.asarray(batch)
				yield batch

	return _iterator(dataset, batch_size, image_crop_operation)
# [END]


# [START Main function that drives the script]
if __name__ == "__main__":
	print(f"Initializing images dataset of {TEST_DATASET_SIZE} images")
	print(
		f"Height - {HEIGHT}, width - {WIDTH} and {COLOR_CHANNELS} channels.\n")

	dataset = initialize_dataset(
		TEST_DATASET_SIZE, HEIGHT, WIDTH, COLOR_CHANNELS)

	print("Dataset has been initialized.\n")

	print(
		f"Initializing croper to select patches of {PATCH_SIZE} x {PATCH_SIZE} \
		from a given image.\n")
	croper = RandomCrop(PATCH_SIZE)
	print("Croper initialized.\n")

	print("Initializing iterator.\n")
	iterator = get_iterator(dataset, BATCH_SIZE, croper)
	print("Irerator initialized.\n")

	print("Starting loop ton go over minibatches produced by the iterator.\n")
	print("Tensors of batches produced have following shapes.\n")
	for mini_batch in iterator:
		print(mini_batch.shape)
	print()
	print("EOS!")
# [END Main function ends]


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
