---
title: pil example imgaug mask augmentation (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imgaug mask augmentation'


## python imgaug mask augmentation

Python pil example: imgaug mask augmentation

```python
# Import PIL
from PIL import Image

# Import segmentation maps from imgaug
from imgaug.augmentables.segmaps import SegmentationMapOnImage

# Open image with mask
pil_mask = Image.open('../input/mask1.png')

# Convert mask to binary map
np_mask = np.array(pil_mask)
np_mask = np.clip(np_mask, 0, 1)

# Create segmentation map
segmap = np.zeros(image.shape, dtype=bool)
segmap[:] = np_mask
segmap = SegmentationMapOnImage(segmap, shape=image.shape)

# Initialize augmentations pipeline
seq = iaa.Sequential([
    iaa.CoarseDropout(0.1, size_percent=0.2), # coarse dropout
    iaa.Affine(rotate=(-30, 30)), # rotate the image
    iaa.ElasticTransformation(alpha=10, sigma=1) # elastc transform
])

# Apply augmentations for image and mask
image_aug, segmap_aug = seq(image=image, segmentation_maps=segmap)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
